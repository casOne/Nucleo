#include "mbed.h"
 
#define REG_TEMP (0xF3) // Temperature Register
#define REG_HUMI (0xF5) // Humidity Register
#define ADDR     (0x40<<1) // address
 
I2C i2c(I2C_SDA, I2C_SCL);
 
DigitalOut myled(LED1);
 
Serial pc(SERIAL_TX, SERIAL_RX);
float TempCelsiusDisplay=0, HumidityDisplay=0;
 
int main()
{
 
    char data_write[2];
    char data_read[3];

    data_write[0] = REG_TEMP;
    data_write[1] = REG_HUMI;
    
    pc.printf("Starting... \r\n");
 
    while (1) {
        // Read temperature register    
        
        int status = i2c.write(ADDR, &data_write[0], 1, 0);
        myled = 0;
        wait(0.1);  // required for processing before reading
        
        i2c.read(ADDR, data_read, 2, 0);

        // Calculate temperature value in Celcius
        int tempval = (int)((int)data_read[0] << 8) | data_read[1];
        tempval &= ~0x0003;
     
        TempCelsiusDisplay = -46.85 + 175.72 / 65536.0 * (float)tempval;

        pc.printf("Temperature = %f", TempCelsiusDisplay);
        
        wait(0.2);

        // Read Humidity register 

        status = i2c.write(ADDR, &data_write[1], 1, 0);
        myled = 0;
        wait(0.1);  // required for processing before reading
        
        i2c.read(ADDR, data_read, 2, 0);

        // Calculate Humidity
        tempval = (int)((int)data_read[0] << 8) | data_read[1];
        tempval &= ~0x0003;
     
        HumidityDisplay = -6.0 + 125.0 / 65536.0 * (float)tempval;

        pc.printf("           Humidity = %f\r\n", HumidityDisplay);

        
        myled = !myled;
        wait(1.0);
    }
 
}
 
