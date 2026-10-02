# ElRobot2-S1 Z module: electrical, v0.6 (one page, first-time builder)

Every number marked **UNVERIFIED** is an assumption until you have measured it on the real parts. Arm servos stay on their own 12 V supply. Only the Z motor, its brake and the Z driver run from 24 V.

## 1. Power chain (24 V side)

```
mains -> [24 V 5 A supply, set 24.0 V] -> [fuse T3.15A] -> [K1 relay contacts] -> VM rail --+-- 470 uF/35 V + 100 nF at the driver
                                                                                            +-- TMC2209 VM/GND -> motor coils A, B
                                                                                            +-- brake coil -> Q1 (low side) -> GND
K1 coil: supply +24 V -> [E-stop mushroom, NC] -> K1 coil -> supply -   (button pressed or any wire broken = K1 drops = VM rail dead)
```
- Supply: Mean Well EDR-120-24 class, 24 V 5 A. Trim to 24.0 V; the TMC2209 limit is 28-29 V and decelerating motors push the rail up, so never trim above 25 V. If you do not want to wire mains, use a sealed 24 V 5 A desk adapter with a barrel plug and a barrel-to-terminal socket instead.
- Wire: 1.0 mm2 for 24 V and motor leads (motor lead 0.5 m, brake lead 0.18 m: extend with a twisted pair, 0.5 mm2). Keep VM/GND leads to the driver under 0.3 m. One ground point at the supply minus; controller GND joins it at the driver.
- Expected load: about 1 A running (UNVERIFIED), brake 154 mA (3.7 W at 24 V, maker's page), so the 5 A supply and T3.15A fuse have wide margin.
- Never connect or disconnect the motor with VM on: it destroys the driver.

## 2. Driver (TMC2209-class module, UART mode)

| Setting | Value | Note |
|---|---|---|
| Microstepping | 16 (interpolated to 256) | 800 microsteps/mm on the 4 mm lead; 40 mm/s = 32 000 steps/s |
| Run current | **1.4 A rms (UNVERIFIED)** = IRUN 24 (1.38 A) or 25 (1.44 A), vsense 0, R_sense 0.11 ohm | I_rms = (CS+1)/32 x 0.325 V/(R_sense+0.02 ohm)/sqrt2; analog Vref alternative: 1.98 V. The motor is rated 2.0 A; the module is only good for about 1.2 A rms without heatsink and airflow (then margin is 2.8 instead of 3.2 at 15-40 mm/s) |
| Hold current | IHOLD 7 (0.44 A rms, UNVERIFIED), IHOLDDELAY 10, TPOWERDOWN 20 | the brake holds the load, so the motor can idle cool |
| Chopper | spreadCycle always (en_spreadCycle = 1) | keeps torque at 600 rpm; stealthChop loses torque at speed |
| Supply | 24.0 V | torque model: margin 3.2 at 15, 25, 40 mm/s, 1.9 at 56 mm/s (24 V); on 12 V only 15 and 25 mm/s work |

## 3. Brake circuit (release only when it is safe, engage on any power loss)

- Brake: power-off (spring-applied) type, 24 V, 3.7 W, 0.7 N*m catalogue (maker's page). **Energised = released.** Unpowered = locked.
- Q1 = logic-level N-MOSFET (IRLZ44N class), drain to brake coil minus, source to GND, gate from the controller pin BRAKE through 100 ohm, **10 kohm gate-to-GND pull-down** so a reset or unplugged controller means "brake locked".
- D1 = 1N5819 (or 1N4007) flyback diode across the coil, cathode to +24 V. Without it Q1 dies on the first turn-off.
- The brake gets 24 V only through K1, so an E-stop, a blown fuse or a mains failure locks the brake with no software involved.
- Sequence to move: EN low (driver on) -> wait 100 ms -> BRAKE high (release) -> wait 80 ms (**UNVERIFIED**, check the release click) -> step pulses. To stop: ramp speed to zero (never set the brake while moving) -> wait 100 ms -> BRAKE low -> wait 150 ms -> EN high (driver off).
- A hard power cut while moving sets the brake at once: up to about 170 N on the nut for 1-2 ms (row BRK in zmod_verification.csv). This is acceptable for a rare E-stop, not for routine stopping.

## 4. Limit switches and E-stop

- Two KW12-3 micro switches (home, top). Wire **normally closed**: COM to GND, NC to the controller input with its pull-up. Healthy and not touched = LOW. Pressed, **or a broken wire**, = HIGH = stop. Add a software limit 2 mm inside each switch.
- E-stop (concept): the mushroom button only drives the relay coil (above), so it carries 40-60 mA, not the motor current. K1 opens the 24 V rail (Z motor + brake). The 12 V arm servo bus is left alive on purpose: cutting it would drop the arm and payload. The controller sees ESTOP_OK (optocoupler across the K1 coil, or a K1 auxiliary contact) go LOW and must command the servos to hold or to stop moving. Alternative if the arm track prefers a full stop: use the second pole of a DPDT K1 on the 12 V line (arm then falls).

## 5. Controller and pins (Raspberry Pi Pico, RP2040, 3.3 V logic; any board with a hardware timer and UART works)

| Pin | Direction | Connects to |
|---|---|---|
| GP2 STEP | out | TMC2209 STEP |
| GP3 DIR | out | TMC2209 DIR |
| GP4 EN | out | TMC2209 EN (low = on); 10 kohm pull-up to 3.3 V keeps the driver off at reset |
| GP0 UART TX / GP1 UART RX | out / in | TMC2209 PDN_UART (TX through 1 kohm, RX direct to the same pin) |
| GP5 BRAKE | out | Q1 gate (100 ohm), 10 kohm pull-down |
| GP6 HOME / GP7 TOP | in, pull-up | limit switches (NC to GND) |
| GP8 ESTOP_OK | in | high = relay K1 closed |
| 3V3 | - | TMC2209 VIO; GND common |

## 6. First power-up (about 30 minutes)

1. Supply alone: measure 24.0 V. Motor and brake unplugged. Add the driver, check 24 V at its VM pins.
2. Plug the motor with VM off, then VM on. Set 0.5 A through UART, turn the screw by hand, confirm both coils pull.
3. Brake test, motor unpowered: power off = screw cannot be turned by hand; apply 24 V to the brake only = turns freely. Then lift the carriage to mid-stroke with the brake **released** and let go: if it creeps down, the thread friction is lower than assumed and the brake is the only holding layer (row D1); never run unattended then.
4. Raise current to 1.4 A. Run 5 strokes at 15 mm/s, then 25, then 40 mm/s (row R1); stop at the first stall or resonance and use 0.75 of the last good speed. Do not go above 40 mm/s.
5. Press the E-stop mid-move once with an empty carriage: the brake must lock, the driver must lose power, and the arm must hold.
