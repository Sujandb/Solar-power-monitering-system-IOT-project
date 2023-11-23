
# SOLAR POWER MONITERING SYSTEM
To create a IoT based Solar monitoring system


**Objectives : -**
•	**Real-Time Monitoring:**  Objective: Provide real-time visibility into the performance of solar panels and the overall solar power system.  Benefits: Enables immediate detection of issues, optimization of energy production, and timely decision-making.
•	**Fault Detection and Diagnostics:**  Objective: Detect and diagnose faults or anomalies in the solar power system.   Benefits: Reduces downtime by allowing prompt identification and resolution of issues, improving overall system reliability.
•	Dust or Soiling Sensors: Optical sensors that can detect the amount of dust or soiling on the surface of the solar panels.
•	Remote Monitoring and Control:
**Remote Access**: Enable remote monitoring and control of solar energy systems, allowing operators to manage and optimize performance from anywhere.    
•	Alerts and Notifications: Provide automated alerts and notifications for system issues or deviations from expected performance, facilitating timely response.
•	User Interface: Provide a user-friendly interface for system operators and end-users to monitor and understand the performance of the solar energy system.


**FEATURES:-**

•	The ESP32 typically comes with a dual-core processor, allowing for efficient multitasking and performance.

•	Integrated Wi-Fi module enables wireless communication, making it suitable for IoT (Internet of Things) applications.

•	The ESP32 is designed to operate with low power consumption, making it suitable for battery-powered devices and applications where power efficiency is crucial.

•	Supports various Wi-Fi protocols (802.11 b/g/n) for wireless communication and Bluetooth protocols for short-range communication.

•	Specifies the range of voltages that the sensor can accurately measure. It's important to choose a module that suits the voltage levels you expect in your application.

•	Specifies the range of current levels that the sensor can accurately measure. It's important to choose a sensor that suits the current levels expected in your application.

•	Servo motors are known for their precise positional control. They can maintain and control their position accurately.

•	Specifies the range of particle sizes that the sensor can detect. Dust sensors may be optimized for specific particle size ranges, such as PM1, PM2.5, or PM10.

•	Indicates how closely the sensor's measurements correspond to the actual particle concentrations. Accuracy is often given as a percentage.

•	The primary feature of a photoresistor is its ability to change resistance in response to changes in illumination. As light intensity increases, the resistance decreases, and vice versa.

•	The primary feature of an LDR is its ability to change resistance based on the intensity of incident light. As the light intensity increases, the resistance decreases, and vice versa.

•	Relays have different contact configurations, including normally open (NO), normally closed (NC), and changeover (CO) or double-throw contacts. These configurations determine how the relay behaves in its normal state and when activated.


MIND MAP :-



![mindmap](https://github.com/Sujandb/Solar-power-monitering-system-IOT-project/assets/109717277/942e4bc4-4ecb-4136-b664-2e084569a203)



WORKING :-

A solar power monitoring system is designed to efficiently track and manage the performance of a solar power installation. The system employs advanced technologies, including real-time monitoring through the integration of Blynk IoT and ESP32 microcontroller. ESP32, a powerful and versatile microcontroller, serves as the brain of the system. It interfaces with the solar panels, inverters, and other relevant components to collect data on parameters such as solar irradiance, temperature, voltage, and current.

The Blynk IoT platform acts as the bridge between the ESP32 and the user interface, enabling real-time visualization and control. Through the Blynk mobile app or web interface, users can monitor the solar power system remotely and receive instant updates on its performance. The system allows users to track energy production, identify potential issues, and optimize the efficiency of the solar installation.

Real-time monitoring is crucial for identifying fluctuations in energy production, detecting faults or malfunctions promptly, and ensuring optimal energy utilization. The ESP32 continuously gathers data from various sensors deployed in the solar power system and transmits this information to the Blynk cloud server. Users can access this data in a user-friendly interface, where customizable widgets display relevant metrics, charts, and graphs.

The integration of Blynk IoT with ESP32 not only enhances real-time monitoring capabilities but also enables remote control and automation features. Users can remotely adjust parameters, such as tilt angles of solar panels or turn on/off specific components, to optimize energy generation based on prevailing conditions. Additionally, the system can send alerts and notifications in case of anomalies, ensuring a proactive approach to maintenance and troubleshooting.

In conclusion, the solar power monitoring system with real-time monitoring using Blynk IoT and ESP32 is a sophisticated solution that empowers users to make informed decisions, maximize energy efficiency, and ensure the longevity of their solar power installations. Through seamless integration and intuitive interfaces, this system contributes to the advancement of sustainable energy practices by providing a comprehensive and user-friendly approach to solar power management.


FLOW CHART :-

![WhatsApp Image 2023-11-17 at 11 00 43 AM](https://github.com/Sujandb/Solar-power-monitering-system-IOT-project/assets/109717277/7e98e8aa-5fdb-486d-a4da-a6469bb870eb)


ALGORITHM :-

![Algorithm](https://github.com/Sujandb/Solar-power-monitering-system-IOT-project/assets/109717277/b6a27b7e-5973-4506-a2cc-d402a6646305)



CIRCUIT BLOCK DIAGRAM :-

![CIRCUITBLOCK](https://github.com/Sujandb/Solar-power-monitering-system-IOT-project/assets/109717277/06fd36d8-592c-4945-938e-259cf658daaa)



CODE : -

#include "BlynkEdgent.h"
#include <Deneyap_Servo.h>
int ldr1 = A0;
int ldr2 = A1;
int pos;
int voltage1 = 12;
int voltage2 = 11;
Servo myservo; 



void servo_ldr()
{
  int  ldr1_read = analogRead(ldr1);
  int  ldr2_read = analogRead(ldr2);
  Serial.print("light1 = ");
  Serial.println(ldr1_read);
  Serial.print("light2 = ");
  Serial.println(ldr2_read);
  int map1 = map(ldr1_read,0,8191,52,0);
  int map2 = map(ldr2_read,0,8191,53,105);
  if(ldr1_read>=ldr2_read)
  {
      myservo.write(map1);
      delay(30);
  }
  else
  {
      myservo.write(map2);
      delay(30);
  } 
}
/*BLYNK_WRITE(V0)
{
  int pinValue = param.asInt();
  digitalWrite(42,pinValue);

}
BLYNK_WRITE(V1)
{
  int pinValue = param.asInt();
  digitalWrite(5,pinValue);

}
BLYNK_WRITE(V2)
{
  int pinValue = param.asInt();
  digitalWrite(6,pinValue);

}*/

void setup()
{
  WiFi.begin(ssid,pass);
  pinMode(ldr1,INPUT);
  pinMode(ldr2,INPUT);
  pinMode(voltage1,INPUT);
  pinMode(voltage2,INPUT);
  Serial.begin(9600);
  myservo.attach(9); 
  delay(100);

  BlynkEdgent.begin();
}

void loop() {
  for(i=0;i<50;i++)
  {
  servo_ldr();
  float volt1 = analogRead(voltage1);
  
  float volt2 = analogRead(voltage2);

  float vout_solar = (volt1/15500)*25;
  float vout_battery = (volt2/15500)*25;
  
  Serial.print("solar voltage = ");
  Serial.println(vout_solar);
  Serial.print("battery voltage = ");
  Serial.println(vout_battery);

  float map_volt1 = map(vout_battery,3.3,4.2,0,100);
  
  Blynk.virtualWrite(V0, vout_solar);
  Blynk.virtualWrite(V1,vout_battery);
  Blynk.virtualWrite(V2, mapping);
  
  //Blynk.virtualWrite(V4, millis() / 1000);
  delay(2000);
  BlynkEdgent.run();
  }
}


OUTPUTS :-

![Screenshot 2023-11-23 181543](https://github.com/Sujandb/Solar-power-monitering-system-IOT-project/assets/109717277/0241fb29-dac7-4b59-a770-8f7d33001b11)
![Screenshot 2023-11-23 174353](https://github.com/Sujandb/Solar-power-monitering-system-IOT-project/assets/109717277/21651719-591f-4670-be9a-34cae73c292d)
![Screenshot 2023-11-23 174305](https://github.com/Sujandb/Solar-power-monitering-system-IOT-project/assets/109717277/8a5aa8c9-e60a-4e0a-82f9-5ee117c2a4a5)

WORKING DEMO VIDEO :-


https://github.com/Sujandb/Solar-power-monitering-system-IOT-project/assets/109717277/081893ee-a9f6-4c46-a034-cc75fde29293


     



