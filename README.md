# CASTRO_LARA_RODRIGUEZ
from maix.ext_dev import imu
import openpyxl

# Inicializar IMU
i = imu.IMU("qmi8658", mode=imu.Mode.DUAL,
                              acc_scale=imu.AccScale.ACC_SCALE_2G,
                              acc_odr=imu.AccOdr.ACC_ODR_8000,
                              gyro_scale=imu.GyroScale.GYRO_SCALE_16DPS,
                              gyro_odr=imu.GyroOdr.GYRO_ODR_8000)

# Crear archivo Excel
wb = openpyxl.Workbook()
ws = wb.active
ws.title = "IMU Data"

# Encabezados en columnas
headers = ["acc_x", "acc_y", "acc_z", "gyro_x", "gyro_y", "gyro_z", "temp"]
ws.append(headers)

# Imprimir encabezados en pantalla
print(f"{headers[0]:>10} {headers[1]:>10} {headers[2]:>10} "
      f"{headers[3]:>10} {headers[4]:>10} {headers[5]:>10} {headers[6]:>10}")

# Recolección de datos (máx 1000)
for n in range(1000):
    data = i.read()
    
    # Guardar en Excel
    ws.append(data)
    
    # Mostrar en columnas alineadas
    print(f"{data[0]:10.4f} {data[1]:10.4f} {data[2]:10.4f} "
          f"{data[3]:10.4f} {data[4]:10.4f} {data[5]:10.4f} {data[6]:10.2f}")

# Guardar archivo
filename = "imu_data.xlsx"
wb.save(filename)

print(f"\nArchivo '{filename}' guardado con 1000 muestras.")
