# Automatic Sensor-Based LED Control System Using STM32

## Aim

To interface a digital sensor with an STM32 microcontroller and automatically control an LED according to the sensor output.

## Apparatus Required

| S. No. | Component | Quantity |
|---:|---|---:|
| 1 | STM32 development board | 1 |
| 2 | Digital sensor or push button | 1 |
| 3 | LED | 1 |
| 4 | 220–330 Ω resistor | 1 |
| 5 | Breadboard | 1 |
| 6 | Jumper wires | As required |
| 7 | USB cable | 1 |

## Algorithm
The sensor continuously detects the required environmental condition. The sensor output is given to an STM32 GPIO input pin. The STM32 processes the sensor signal. If the programmed condition is satisfied, STM32 sets the LED GPIO HIGH. Otherwise, it sets the LED GPIO LOW. Thus, the LED operates automatically without manual switching.



## Program

#include "main.h" 
void SystemClock_Config(void); static void MX_GPIO_Init(void); 
int main(void) { GPIO_PinState sensor_state; 
/* Initialize HAL Library */ 
HAL_Init(); 
/* Configure System Clock */ 
SystemClock_Config();
/* Initialize GPIO */

MX_GPIO_Init(); 
while (1) {    
    /* Read sensor input from PA0 */    
    sensor_state = HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0);    
    /* Check sensor condition */    
    if (sensor_state == GPIO_PIN_SET){        
          /* Sensor HIGH - Turn ON LED */        
          HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_SET);    
     }    
     else{        
             /* Sensor LOW - Turn OFF LED */        
             HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_RESET);    
      } 
  }
}

/------------------------------------------------ GPIO Initialization ------------------------------------------------/ static void 
MX_GPIO_Init(void) { GPIO_InitTypeDef GPIO_InitStruct = {0};

    /* Enable GPIOA Clock */ 
    __HAL_RCC_GPIOA_CLK_ENABLE(); 
    
    /* Configure PA0 as Digital Input */ 
    GPIO_InitStruct.Pin = GPIO_PIN_0; 
    GPIO_InitStruct.Mode = GPIO_MODE_INPUT; 
    GPIO_InitStruct.Pull = GPIO_PULL_DOWN; 
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct); 
    /* Configure PA5 as Digital Output */
    GPIO_InitStruct.Pin = GPIO_PIN_5; 
    GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP; 
    GPIO_InitStruct.Pull = GPIO_NOPULL; 
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
    HAL_GPIO_Init(GPIOA, &GPIO_InitStruct); 
    
    /* Initially turn OFF LED */ 
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_RESET); 
}

/------------------------------------------------ System Clock Configuration ------------------------------------------------/ void SystemClock_Config(void) { RCC_OscInitTypeDef RCC_OscInitStruct = {0}; RCC_ClkInitTypeDef RCC_ClkInitStruct = {0}; 
      /* Configure MSI Oscillator */ 
      RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_MSI;
      RCC_OscInitStruct.MSIState = RCC_MSI_ON; 
      RCC_OscInitStruct.MSIClockRange = RCC_MSIRANGE_6;
      RCC_OscInitStruct.MSICalibrationValue = 0; 
      
      HAL_RCC_OscConfig(&RCC_OscInitStruct); 
      /* Configure CPU, AHB and APB clocks */ 
      RCC_ClkInitStruct.ClockType = 
            RCC_CLOCKTYPE_HCLK |
            RCC_CLOCKTYPE_SYSCLK |
            RCC_CLOCKTYPE_PCLK1; 
            
      RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_MSI;
      RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
      RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1; 
      
      HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0); 
}




## Output

<img width="525" height="528" alt="image" src="https://github.com/user-attachments/assets/1307fb60-0661-4a70-b9a4-c3a11266d8dc" />




## Result

The digital sensor was successfully interfaced with the STM32 microcontroller. The LED connected to `PA5` turned ON when the sensor input at `PA0` was HIGH and turned OFF when the sensor input was LOW.
