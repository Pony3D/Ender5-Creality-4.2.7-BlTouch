# Ender5-Creality-4.2.7-BlTouch
Configuracion de BlTouch para Ender5 con placa Creality 4.2.7

1)3D Touch y BlTouch

Debido a que he encontrado muy mala informacion de como configurar marlin para esta placa, y que los exemplos vienen cambiados, mucha gente utiliza el Z_MIN_PIN como tigger del sensor touch, pero para este fin tenemos configurados unos pines que debemos seleccionar, esta es la configuracion adecuada para ello.
En las imagenes podeis ver donde va conectado directamente en la placa.

NOTA: Los colores de los cables pueden variar, pero se suele respetar el rojo y el amarillo como voltajes.


2) Configurando Marlin

Tomando el punto de partida, que vamos a configurar todo como lo tengo yo conectado, debemos hacer las siguientes modificaciones:

2.1) En Configuration.h

Des comentamos los siguientes parametros:

#define USE_ZMIN_PLUG

#define USE_PROBE_FOR_Z_HOMING

#define BLTOUCH

#define AUTO_BED_LEVELING_BILINEAR

#define NOZZLE_TO_PROBE_OFFSET { -42, -10, 0 }  //La posición dependerá de vuestra impresora, estos son mis valores).

#define RESTORE_LEVELING_AFTER_G28

#define GRID_MAX_POINTS_X 9 //9 es el numero de puntos de la cama sobre los que hacer medición, pueden ser mas o menos.

#define GRID_MAX_POINTS_Y GRID_MAX_POINTS_X

#define Z_MIN_PROBE_PIN PB1

PB1 ---> Este es el puerto del procesador STM32F103, (Puerto numerico numero 27) puerto donde va el cable blanco de nuestro Touch. He aquí la diferencia.




2.2) En Configuration_adv.h

#define BABYSTEPPING

#if ENABLED(BABYSTEPPING)

  //#define INTEGRATED_BABYSTEPPING         // EXPERIMENTAL integration of babystepping into the Stepper ISR
  
  //#define BABYSTEP_WITHOUT_HOMING
  
  //#define BABYSTEP_XY                     // Also enable X/Y Babystepping. Not supported on DELTA!
  
  #define BABYSTEP_INVERT_Z false           // Change if Z babysteps should go the other way
  
  //#define BABYSTEP_MILLIMETER_UNITS       // Specify BABYSTEP_MULTIPLICATOR_(XY|Z) in mm instead of micro-steps
  
  #define BABYSTEP_MULTIPLICATOR_Z  1       // (steps or mm) Steps or millimeter distance for each Z babystep
  
  #define BABYSTEP_MULTIPLICATOR_XY 1       // (steps or mm) Steps or millimeter distance for each XY babystep
  

  #define DOUBLECLICK_FOR_Z_BABYSTEPPING    // Double-click on the Status Screen for Z Babystepping.
  
  #if ENABLED(DOUBLECLICK_FOR_Z_BABYSTEPPING)
  
    #define DOUBLECLICK_MAX_INTERVAL 1250   // Maximum interval between clicks, in milliseconds.
    
                                            // Note: Extra time may be added to mitigate controller latency.
                                            
    #define BABYSTEP_ALWAYS_AVAILABLE     // Allow babystepping at all times (not just during movement).
    
    //#define MOVE_Z_WHEN_IDLE              // Jump to the move Z menu on doubleclick when printer is idle.
    
    #if ENABLED(MOVE_Z_WHEN_IDLE)
    
      #define MOVE_Z_IDLE_MULTIPLICATOR 1   // Multiply 1mm by this factor for the move step size.
      
    #endif
    
  #endif

  //#define BABYSTEP_DISPLAY_TOTAL          // Display total babysteps since last G28

  #define BABYSTEP_ZPROBE_OFFSET          // Combine M851 Z and Babystepping
  
  #if ENABLED(BABYSTEP_ZPROBE_OFFSET)
  
    //#define BABYSTEP_HOTEND_Z_OFFSET      // For multiple hotends, babystep relative Z offsets
    
    #define BABYSTEP_ZPROBE_GFX_OVERLAY   // Enable graphical overlay on Z-offset editor
    
  #endif
  
#endif

NOTA: Los tutoriales te diran que des comentes #define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN, pero no lo hagas, esto seria para utilizar el puerto original del sensor mecanico como el nuevo sensor bltouch, cosa que no queremos.


3) Ajuste final.

Solo nos queda compilar nuestro marlin y subirlo a nuestra Ender 5, como es un procesador de 32bits, no utilizamos Arduino para esto.
Ahora ya podemos entrar en el menú de nuestra impresora y hacer un HOME a todos los ejes, yo siempre tengo un dedo sobre el botón de apagado... manias de los que tenemos Tronxy's... 🤣🤣 y veremos como nos realiza un home perfecto, ou yeah man 😎.

Si habeis seguido los pasos y configurado todo correctamente, al dar una doble pulsación sobre el botón de la pantalla de la Ender 5, deberiais entrar directamente en el menú de babystepping, donde configurar el desfase de Z.
No olvideis pulsar Guardar EEPROM!
