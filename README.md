# PV-in-MG-with-BLDC Cm36-3650 
Solar Panel in Marine Gliders with BLDC
A brushless DC (BLDC) motor control system integrated with a solar photovoltaic (PV) and 
battery energy storage system in a Simulink environment is the subject of this work  thesis.  
Early simulations showed extreme instability, with divergent torque and speed signals, especially 
when trying to control speeds that were higher than what the motor could physically achieve. 
By carefully adjusting the parameters, especially the observer poles and PID controller 
gains, stable operation was achieved with a 2 Nm load at a practical reference speed of 250 
RPM. The battery's energy and charge characteristics, including its nominal voltage, capacity, 
and charge/discharge current limits, as well as the solar panel's power generation capabilities, 
were crucial for system power delivery. 
Data shows that the solar panel with a capacity of 300 W can power the engine for 
approximately 12 hours at 250 RPM and 2 Nm after fully charging the 943.5 Wh battery, 
which takes around 4.4 hours. This report further explains motor physical system limitations, 
control loop adjustment, and the difference between load torque (Nm) vs mass (kg) applies 
here. It also offers a detailed comparison while operating with a greater torque load of 13 Nm.
efficient control systems for BLDC motors is crucial, 
especially in hybrid energy setups that mix solar panels with battery storage. The aim of this 
study was to analyze and stabilize a Simulink model of the system, which initially faced 
significant challenges with control stability and adherence to the physical constraints of the 
motor. The system was modeled using  Matlab /Simulink, integrating a BLDC motor, a gearbox, a 
solar PV 300W, a boost converter, a battery bank, and a Battery Management System (BMS). 
The motor control utilized a sensorless observer for speed and position estimation, coupled 
with cascaded PID controllers for speed and current regulation. System parameters were 
defined and managed via a MATLAB script, allowing for systematic modification and 
analysis. As the motor is sensorless uses back EMF signal.
Initial parameters, including motor characteristics (e.g., rated voltage, no-load speed, 
electrical constants), gearbox ratio, and control gains (Proportional-Integral-Derivative (PID) 
gains for speed and current loops, and observer poles), were iteratively refined. The primary 
diagnostic tools were Simulink scopes, visualizing reference speed, estimated rotor speed, 
and measured torque.

#  Initial Instability at High Speed Targets 
Early simulations, particularly when attempting to achieve a 3000 RPM output speed with a 
2:1 gearbox (requiring 6000 RPM from the motor), consistently resulted in severe instability. 
The "Reference and estimated rotor speed" plots showed the estimated speed diverging to 
extremely high values. This behavior was primarily attributed to physical motor limitation.
The motor parameters initially defined (e.g., a 24V motor  with a 3000 RPM no-load speed) 
were physically incapable of reaching the  commanded 6000 RPM required by the gearbox for 
the 3000 RPM output target. The observer's pole configuration, specifically the presence 
of a positive pole in Lobs (e.g., [-35000 2000]), caused its internal state estimation to 
diverge, leading to inaccurate feedback for the controllers. 
Aggressive PID Gains aplied. The initial speed control PID gains (SpeedCtrl_P = 10, 
SpeedCtrl_I = 20, SpeedCtrl_D = 0.5) were excessively high, contributing to 
oscillatory and unstable responses. 
#  Stable Operation at  Speed (250 RPM Output, 2 Nm Load) 
Following a series of parameter adjustments, stable operation was successfully achieved at a 
reduced, physically achievable output speed target. With the 
BLDC_MOTOR_RPM_TARGET set to 250 RPM (meaning the motor aims for 500 RPM, 
well within its 250 RPM no-load capability) and a Tload of 2 Nm, the system demonstrated 
stable tracking. 
The key parameter changes that led to this stabilization included: 
Motor Parameters: The bldcMotor parameters were adjusted to reflect a motor with a 
250 RPM no-load speed at 24V, including recalculated kV_RPM_per_V and 
backEMFConstant_V_per_rad_s, and adjusted terminalResistance_Ohm, 
terminalInductance_H, and poles to be more typical for such a low-speed motor. 
Observer Stability: The observer poles (bldcControl.observerPoles and Lobs) were 
corrected to be entirely negative (e.g., [-1000 -500]), ensuring the observer's stability 
and accurate state estimation. 
Conservative PID Gains: The speed control PID gains (bldcControl.speedKp = 0.1, 
bldcControl.speedKi = 0.02, bldcControl.speedKd = 0.0005) and current loop gains 
(bldcControl.currentKp = 0.6, bldcControl.currentKi = 30) were significantly reduced 
to more conservative values, preventing oscillations and saturation. 
The plot in clearly illustrates the estimated rotor speed (blue line) smoothly and accurately 
tracking the 250 RPM reference (yellow line). The measured torque also exhibited a stable 
response after an initial transient, indicating effective load management. 
Solar PV System Characteristics 
The solar photovoltaic (PV) system serves as the primary renewable energy source for the 
hybrid system. It is configured with a single 300 W panel. Key electrical characteristics of 
this panel include an open-circuit voltage (Voc) of 39 V and a short-circuit current (Isc) of 
9.80 A. At its maximum power point (MPP), the panel delivers 32.5 V and 9.23 A. The 
panel's efficiency is specified at 20.30%. 
The solar panel's power output is highly dependent on environmental factors such as 
irradiance (set at 1000 W/m$^2$ for standard test conditions) and temperature. The system 
incorporates Maximum Power Point Tracking (MPPT) to ensure the PV array operates at its 
optimal power output, feeding energy into the common DC bus via a boost converter. The 
solar panel's contribution directly influences the charging capability of the battery and the 
direct power supply to the motor.

#  Battery Energy and Charge Characteristics 
The battery pack, central to the hybrid energy system, provides energy storage and load 
balancing capabilities. It is comprised of Li-ion cells, with each cell having a nominal voltage 
of 3.7 V. The entire pack has a nominal voltage of 74 V, indicating a series configuration of 
20 cells. The battery pack boasts a total capacity of 12.75 Ah (12750 mAh), equating to a 
total energy storage of 943.5 Wh. 
This energy capacity dictates the total energy available to the motor and other loads over 
time. The battery's ability to supply or absorb power instantaneously is governed by its 
maximum charge and discharge current limits. These were set at 12.75 A (1C rate) for 
charging and 25.5 A (2C rate) for discharging. These limits are crucial as they govern the 
instantaneous power that can be drawn from or supplied to the battery, directly impacting the 
motor's performance under varying load conditions and ensuring the battery operates within 
safe parameters. A Battery Management System (BMS) actively manages these charge and 
discharge processes.

The 300 W solar panel can  fully charge the 943.5 Wh battery in approximately 4.4 hours, and this charged battery 
can then power the BLDC motor at 250 RPM and 2 Nm load for about 12 hours.

 #  Conclusion 
The BLDC motor control system, when configured with physically realistic motor parameters 
and appropriately tuned control gains, demonstrates stable and accurate speed tracking at 
achievable operating points. The prior instability was primarily due to attempting to operate 
the motor beyond its physical speed limits and using unstable control parameters. The successful stabilization 
at 250 RPM with a 2 Nm load validates the importance of proper system 
characterization and meticulous control system tuning. For applications requiring higher output 
speeds (e.g., 3000 RPM), a motor with significantly higher inherent speed capabilities would 
be necessary. While the current 250 RPM motor is physically capable of producing 13 Nm of 
torque, achieving stable operation at this higher load would require careful consideration of the 
entire power system's current capacity and potential further tuning of the control loops, with 
the battery's energy and charge limits, as well as the solar panel's power contribution, being 
critical factors.
