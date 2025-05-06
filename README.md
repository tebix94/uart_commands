This repository is an CubeIDE project developed by Esteban Torres for the 3rd assignment of the "ADMON DE TAREAS EN SIST. EMBEBIDOS Y TIEMPO REAL" course from Dr. Hirales.

Projects works on STMicroelectronics Nucleo-144 F767ZI development board. As shown in the .ioc file it uses default USART3 which is physically attached to the ST-Link board USB port.

Project simply uses the HAL midware functions to listen to the serial channel. The purpose is to connect the board to a PC which will send the following commands:

LED:01,STATE:ONX -> turns on the LED labeled as LD1 on the board.
LED:01,STATE:ONX -> turns off the LED labeled as LD1 on the board.

LED:02,STATE:ONX -> turns on the LED labeled as LD2 on the board.
LED:02,STATE:ONX -> turns off the LED labeled as LD2 on the board.

LED:03,STATE:ONX -> turns on the LED labeled as LD3 on the board.
LED:03,STATE:ONX -> turns off the LED labeled as LD3 on the board.

To reduce code, the whole command message is not processed character by character, MCU only looks for the bytes that contents the LED number and the desired state.

        led_number = rx_buffer[5] - 48; // Extract LED number -> number zero has a value of 48 in the ascii table, subsequentes values (49, 50, 51 and so on) corresponds to the subsequents numbers (1, 2, 3 and so on). So substracting 48 is the easiest solution.
	      status[0] = rx_buffer[13]; // Extract status 1st character
	      status[1] = rx_buffer[14]; // Extract status 2nd character
	      status[2] = rx_buffer[15]; // Extract status 3rd character
                                                                  //--------> as you can see it expects 3 characters as the state, so UART reading is not dynamic :,(

Message is captures at the HAL UART callback function only, the message is processed at the main loop.

The board will echo the commands after being received just for debugging purpose, other than that, please ignore it.
