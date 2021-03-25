from scipy.io import loadmat
import numpy as np
import matplotlib.pyplot as plt

# region Data

# .mat Location
path = r'D:\Downloads\simulacoes'
mot_a = loadmat(path + '\simulacao_motorista_A')
mot_b = loadmat(path + '\simulacao_motorista_B')

# a, b and c are coefs to read the data
a = len(mot_a['chfr_out_time'])
b = len(mot_b['chfr_out_time'])
c = a + b

# V is the string with the names of variables
v = [('chfr_out_time'),('chfr_out_distance_m'), ('chfr_out_speed_kph'), ('drv_out_accelPedal_nu'),
('drv_out_brakePedal_nu'), ('drv_out_activateCC_nu'), ('ems_out_fuelMass_kg'), ('engplnt_out_speed_rpm'), ('re_out_actAltitude_m'),
('re_out_roadInclination_pct'), ('tms_out_currentGear_nu')]

# Reading..
var = np.zeros([len(v), (a+b)])
for i in range(len(v)):
        var[i,:] = np.concatenate((mot_a[v[i]], mot_b[v[i]])).reshape(c)
# endregion

# region Dist plot
plt.style.use('seaborn')
plt.plot(var[0, :a]/60, var[1, :a]/1000, label = 'Driver a')
plt.plot(var[0, a:]/60, var[1, a:]/1000, label = 'Driver b')
plt.xlabel('Time [h]')
plt.ylabel('Distance [Km]')
plt.legend()
plt.show()
# endregion

# region Fuel plot
plt.style.use('seaborn')
plt.plot(var[0, :a]/60, var[6, :a], label = 'Driver a')
plt.plot(var[0, a:]/60, var[6, a:], label = 'Driver b')
plt.title('Fuel Consumption')
plt.xlabel('Time [min]')
plt.ylabel('Fuel [Kg]')
plt.legend()
plt.show()
# endregion

# region Vel plot
plt.style.use('seaborn')
plt.plot(var[0, :a]/60, var[2, :a], label = 'Driver a')
plt.plot(var[0, a:]/60, var[2, a:], label = 'Driver b')
plt.title('Truck Speed')
plt.xlabel('Time [min]')
plt.ylabel('Speed [K/h]')
plt.legend()
plt.show()
# endregion

# region Vel_med bars
t_min = np.linspace(10,60,6)
vel_med_a = np.zeros(6)
vel_med_b = np.zeros(6)
dt = 0.1
for i in range(6):
        index0 =int(600*(i)/dt)
        index = int(600*(i+1)/dt)
        vel_med_a[i] = np.average(var[2, index0:index])
        vel_med_b[i] = np.average(var[2, (a+index0): (a+index)])

s = 3
plt.bar(t_min-(s/2), vel_med_a, width = s, align = 'center', label = 'Driver a')
plt.bar(t_min+(s/2), vel_med_b, width = s, align = 'center', label = 'Driver b')
plt.xlabel('Time [min]')
plt.ylabel('Speed [K/h]')
plt.title('Average Speed')
plt.legend()
plt.show()
# endregion

# region Vel_med _ac plot
vel_med_ac = np.zeros(len(var[2,:a]))
rot_med_ac = np.zeros(len(var[2,:a]))
for i in range(len(var[2,:a])):
         vel_med_ac[i] = np.average(var[2, :i])
         rot_med_ac[i] = np.average(var[7, :i])

vel_med_bc = np.zeros(len(var[2,a:]))
rot_med_bc = np.zeros(len(var[2,a:]))
for i in range(len(var[2,a:])):
         vel_med_bc[i] = np.average(var[2, a:(a+i)])
         rot_med_bc[i] = np.average(var[7, a:(a + i)])

plt.plot(var[0,:a]/60,vel_med_ac, label = 'Driver a')
plt.plot(var[0,a:]/60,vel_med_bc, label = 'Driver b')
plt.xlabel('Time [min]')
plt.ylabel('Speed [K/h]')
plt.title('Accumulated Average Speed')
plt.legend()
plt.show()
# endregion

# region Rot plot
plt.plot(var[0, :a]/60, var[7, :a], label = 'Driver a')
plt.plot(var[0, a:]/60, var[7, a:], label = 'Driver b')
plt.title('Motor Rotation')
plt.xlabel('Time [min]')
plt.ylabel('Rotation [rpm]')
plt.legend()
plt.show()
# endregion

# region Rot_med plot
plt.plot(var[0, :a]/60, rot_med_ac, label = 'Driver a')
plt.plot(var[0, a:]/60, rot_med_bc, label = 'Driver b')
plt.title('Accumulated Average Motor Rotation')
plt.xlabel('Time [min]')
plt.ylabel('Rotation [rpm]')
plt.legend()
plt.show()
# endregion

# region gear plot
plt.style.use('seaborn')
plt.plot(var[0, :a]/60, var[10, :a], label = 'Driver a')
plt.plot(var[0, a:]/60, var[10, a:], label = 'Driver b')
plt.title('Gear')
plt.xlabel('Time [min]')
plt.ylabel('Gear')
plt.legend()
plt.show()
# endregion

# region Gear bars

gear = 13
time_gear_a = np.zeros(gear)
time_gear_b = np.zeros(gear)

for j in range(gear):
        for i in range(len(var[10, :a])):
                if var[10, i] == j:
                        time_gear_a[j] = time_gear_a[j] + dt
for j in range(gear):
        for i in range(len(var[10, a:])):
                if var[10, i] == j:
                        time_gear_b[j] = time_gear_b[j] + dt
gear1 = np.linspace(0,12,13)
plt.bar(gear1-(0.3/2), time_gear_a/60, width = 0.3, align = 'center', label = 'Driver a')
plt.bar(gear1+(0.3/2), time_gear_b/60, width = 0.3, align = 'center', label = 'Driver b')
plt.xlabel('Gear')
plt.ylabel('Time [min]')
plt.title('Gear Time')
plt.legend()
plt.show()
# endregion

# region Acel plot
acel_a = np.diff(var[3, :a])
acel_b= np.diff(var[3,a:])
plt.plot(var[0, :(a-1)]/60, acel_a, label = 'Driver a')
plt.plot(var[0, a:-1]/60, acel_b, label = 'Driver b')
plt.title('Diff_Aceleration')
plt.xlabel('Time [min]')
plt.ylabel('Diff_Aceleration [%]')
plt.ylim(0,)
plt.legend()
plt.show()
# endregion

# region Auto_pil plot
plt.style.use('seaborn')
plt.plot(var[0, :a]/60, var[5, :a], label = 'Driver a')
plt.plot(var[0, a:]/60, var[5, a:], label = 'Driver b')
plt.title('Automatic Pilot')
plt.xlabel('Time [min]')
plt.ylabel('Use')
plt.legend()
plt.show()

print('Auto_pil_a t = ', np.count_nonzero(var[5, :a]>0))
print('Auto_pil_b t = ', (np.count_nonzero(var[5, a:]!=0)*dt))
# endregion

# region brake plot
brake_a = var[4, :a]
brake_b= var[4,a:]
plt.plot(var[0, :(a)]/60, brake_a, label = 'Driver a')
plt.plot(var[0, a:]/60, brake_b, label = 'Driver b')
plt.title('Brake Use')
plt.xlabel('Time [min]')
plt.ylabel('Brake [%]')
plt.ylim(0,)
plt.legend()
plt.show()
print('Average brake use A = ', np.mean(brake_a))
print('Average brake use B= ', np.mean(brake_b))
# endregion

# region Road plot
plt.style.use('seaborn')
plt.subplot(2,1,1)
plt.plot(var[0, :a]/60, var[8, :a], label = 'Driver a')
plt.plot(var[0, a:]/60, var[8, a:], label = 'Driver b')
plt.ylabel('Level [m]')
plt.legend()
plt.subplot(2,1,2)
plt.plot(var[0, :a]/60, var[9, :a], label = 'Driver a')
plt.plot(var[0, a:]/60, var[9, a:], label = 'Driver b')
plt.ylabel('Runway Slope [°]')
plt.xlabel('Time [min]')
plt.legend()
plt.show()
# endregion

