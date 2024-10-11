Comandos HTTP básicos;
Es una API Jim, pero no como la conocemos.

La WebUI y el servidor de la cámara se comunican completamente a través de solicitudes y respuestas HTTP; 
esto hace posible controlar todas las funciones de la cámara mediante solicitudes GET. Una API en vigor.
URI
Puerto http
/ - Índice predeterminado
/?view=full|simple|portal -  Ir directamente al índice específico
/capture - Devolver una imagen instantánea Jpeg
/status - Devuelve una cadena JSON con todos los estados/pares de la cámara enumerados
/control?var=<key>&val=<val> - Cambiar <key> to <val>
/dump - Página de estado
/stop - Finalizar todas las transmisiones activas
Puerto de transmisión
/ - Raw stream  Corriente sin procesar
/view - Visor de transmisiones
key / val Visor de transmisiones


Llame al /estado URI para recibir una respuesta JSON que contiene todas las configuraciones disponibles y el valor actual.

Llamar /control?var=<clave>&val=<valor> con una clave de configuración y un valor para establecer las propiedades de la cámara o activar acciones.
Ajustes
lámpara: valor de la lámpara en porcentaje; entero, 0 - 100 (-1 = deshabilitado)

tamaño del marco - Ver más abajo

min_frame_time: duración mínima del fotograma en ms; Se utiliza para limitar los FPS máximos. Debe ser un entero positivo

calidad: 10 a 63 (ov3660: 4 a 10)

contraste - -2 a 2 (ov3660: -3 a 3)

brillo - -2 a 2 (ov3660: -3 a 3)

saturación - -2 a 2 (ov3660: -4 a 4)

nitidez - (ov3660: -3 a 3)

eliminar ruido - (ov3660: 0 a 8)

ae_level - (ov3660: -5 a 5)

efecto_especial - 0=Sin efecto, 1=Negativo, 2=Escala de grises, 3=Tono rojo, 4=Tono verde, 5=Tono azul, 6=Sepia

awb - 0 = deshabilitar, 1 = habilitar

awb_gain - 0 = deshabilitar, 1 = habilitar

wb_mode - si awb está habilitado: 0=Auto, 1=Soleado, 2=Nublado, 3=Oficina, 4=Casa

aec - 0 = deshabilitar, 1 = habilitar

aec_value: 0 a 1200 (ov3660: 0 a 1536)

aec2 - 0 = deshabilitar, 1 = habilitar

ae_level - -2 a 2 (no ov3660)

agc - 0 = deshabilitar, 1 = habilitar

agc_gain - 0 a 30 (ov3660: 0 a 64)

techo alto - 0 a 6 (ov3660: 0 a 511)

bpc - 0 = deshabilitar, 1 = habilitar

wpc - 0 = deshabilitar, 1 = habilitar

raw_gma - 0 = deshabilitar, 1 = habilitar

lenc - 0 = deshabilitar, 1 = habilitar

hmirror - 0 = deshabilitar, 1 = habilitar

vflip - 0 = deshabilitar, 1 = habilitar

rotar - Ángulo de rotación; entero, sólo se reconocen los valores -90, 0, 90

dcw - 0 = deshabilitar, 1 = habilitar

barra de colores: superpone un patrón de prueba de color en la secuencia; entero, 1 = habilitado

face_detect - Detección de rostros; 1 = habilitado, solo configurable si tamaño de fotograma <= 4 (CIF)

face_recognize - Reconocimiento facial; 1 = habilitado, solo configurable si la detección de rostros ya está habilitada
Solo lectura
Estos valores se devuelven en el /estado Respuesta JSON, pero no se puede configurar mediante el /control TIPO.

cam_name - Nombre de la cámara; Cadena

code_ver: fecha y hora de compilación del código; Cadena

stream_url: URL de transmisión sin formato; cadena
Valores de tamaño de fotograma
Estos pueden variar entre las diferentes versiones del marco ESP.

 0 - PULGAR (96x96)

 1 - QQVGA (160x120)

 3 - HQVGA (240x176)

 5 - QVGA (320x240)

 6-CIF (400x296)

 7-HVGA (480x320)

 8-VGA (640x480)

 9-SVGA (800x600)

10-XGA (1024x768)

11 - Alta definición (1280x720)

12 - SXGA (1280x1024)

13 - UXGA (1600x1200)

Sólo para módulos de cámara de 3Mp+:

14 - Full HD (1920x1080)

17 - QXGA (2048x1536)
Comandos
Estos son comandos; se pueden enviar llamando al /control URI con ellos como el <clave> (a <valor> también se debe proporcionar, 
pero puede tener cualquier valor y se ignora).

face_enroll: registra una nueva cara en FaceDB (solo cuando el reconocimiento facial está activo)

save_prefs: guarda el archivo de preferencias

clear_prefs: elimina el archivo de preferencias

reiniciar: reinicia la cámara
Ejemplos
Luz de flash: encendido/medio/apagado
http://<DIRECCIÓN-IP>/control?var=lámpara&val=100
http://<DIRECCIÓN-IP>/control?var=lámpara&val=50
http://<DIRECCIÓN-IP>/control?var=lámpara&val=0
Establecer resolución en VGA
http://<DIRECCIÓN-IP>/control?var=framesize&val=8
Mostrar detalles y configuraciones de la cámara
Todas las configuraciones se devuelven mediante un solo estado llamar JSON formato.
http://<DIRECCIÓN-IP>/status
Devoluciones:   {"lámpara":0,"autolamp":0,"min_frame_time":0,"framesize":9,"calidad":10,"xclk":8,"brillo":0,"contraste":0," saturación":0,"nitidez":0,"efecto_especial":0,"wb_mode":0,"awb":1,"awb_gain":1,"aec":1,"aec2":0,"ae_level" :0,"aec_value":204,"agc":1,"agc_gain":0,"gainceiling":0,"bpc":0,"wpc":1,"raw_gma":1,"lenc":1 ,"vflip":1,"hmirror":1,"dcw":1,"colorbar":0,"cam_name":"Cámara de prueba ESP32","code_ver":"10 de marzo de 2022 a las 14:00:45" ,"rotate":"0","stream_url":"http://10.0.0.181:81/"}
Reinicia la cámara
http://<DIRECCIÓN-IP>/control?var=reboot&val=0

Puede probarlos usted mismo en la barra de direcciones del navegador, desde la línea de comandos con rizo y compañía. o úselos programáticamente desde el lenguaje de programación de su elección.

----------------------------------
Basic HTTP Commands;
It's an API Jim, but not as we know it

The WebUI and camera server communicate entirely via HTTP requests and responses; this makes controlling all functions of the camera via GET requests possible. An API in effect.
URI's
Http Port
/ - Default index
/?view=full|simple|portal - Go direct to specific index
/capture - Return a Jpeg snapshot image
/status - Returns a JSON string with all camera status / pairs listed
/control?var=<key>&val=<val> - Set <key> to <val>
/dump - Status page
/stop - End all active streams
Stream Port
/ - Raw stream
/view - Stream viewer
key / val settings and commands
Call the /status URI to recieve a JSON response containing all the available settings and current value.

Call /control?var=<key>&val=<val> with a settings key and value to set camera properties or trigger actions.
Settings
lamp            - Lamp value in percent; integer, 0 - 100 (-1 = disabled)

framesize       - See below

min_frame_time  - Minimal frame duration in ms; used to limit max FPS. Must be positive integer

quality         - 10 to 63 (ov3660: 4 to 10)

contrast        - -2 to 2 (ov3660: -3 to 3)

brightness      - -2 to 2 (ov3660: -3 to 3)

saturation      - -2 to 2 (ov3660: -4 to 4)

sharpness       - (ov3660: -3 to 3)

denoise         - (ov3660: 0 to 8)

ae_level        - (ov3660: -5 to 5)

special_effect  - 0=No Effect, 1=Negative, 2=Grayscale, 3=Red Tint, 4=Green Tint, 5=Blue Tint, 6=Sepia

awb             - 0 = disable, 1 = enable

awb_gain        - 0 = disable, 1 = enable

wb_mode         - if awb enabled: 0=Auto, 1=Sunny, 2=Cloudy, 3=Office, 4=Home

aec             - 0 = disable, 1 = enable

aec_value       - 0 to 1200 (ov3660: 0 to 1536)

aec2            - 0 = disable, 1 = enable

ae_level        - -2 to 2 (not ov3660)

agc             - 0 = disable, 1 = enable

agc_gain        - 0 to 30 (ov3660: 0 to 64)

gainceiling     - 0 to 6 (ov3660: 0 to 511)

bpc             - 0 = disable, 1 = enable

wpc             - 0 = disable, 1 = enable

raw_gma         - 0 = disable, 1 = enable

lenc            - 0 = disable, 1 = enable

hmirror         - 0 = disable, 1 = enable

vflip           - 0 = disable, 1 = enable

rotate          - Rotation Angle; integer, only -90, 0, 90 values are recognised

dcw             - 0 = disable, 1 = enable

colorbar        - Overlays a color test pattern on the stream; integer, 1 = enabled

face_detect     - Face Detection; 1 = enabled, Only settable if framesize <= 4 (CIF)

face_recognize  - Face recognition; 1 = enabled, only settable if Face detection is already enabled
Read Only
These values are returned in the /status JSON response, but cannot be set via the /control URI.

cam_name        - Camera Name; String

code_ver        - Code compile date and time; String

stream_url      - Raw stream URL; string
Framesize values
These may vary between different ESP framework releases

 0 - THUMB (96x96)

 1 - QQVGA (160x120)

 3 - HQVGA (240x176)

 5 - QVGA (320x240)

 6 - CIF (400x296)

 7 - HVGA (480x320)

 8 - VGA (640x480)

 9 - SVGA (800x600)

10 - XGA (1024x768)

11 - HD (1280x720)

12 - SXGA (1280x1024)

13 - UXGA (1600x1200)

Only for 3Mp+ camera modules:

14 - FHD (1920x1080)

17 - QXGA (2048x1536)
Commands
These are commands; they can be sent by calling the /control URI with them as the <key> (a <val> must also be supplied, 
but can be any value and is ignored).

face_enroll     - Enroll a new face in the FaceDB (only when face recognition is avctive)

save_prefs      - Saves preferences file

clear_prefs     - Deletes the preferences file

reboot          - Reboots the camera
Examples
Flash light: on/mid/off
http://<IP-ADDRESS>/control?var=lamp&val=100
http://<IP-ADDRESS>/control?var=lamp&val=50
http://<IP-ADDRESS>/control?var=lamp&val=0
Set resolution to VGA
http://<IP-ADDRESS>/control?var=framesize&val=8
Show camera details and settings
All settings are returned via single status call in JSON format.
http://<IP-ADDRESS>/status
Returns:   {"lamp":0,"autolamp":0,"min_frame_time":0,"framesize":9,"quality":10,"xclk":8,"brightness":0,"contrast":0,"saturation":0,"sharpness":0,"special_effect":0,"wb_mode":0,"awb":1,"awb_gain":1,"aec":1,"aec2":0,"ae_level":0,"aec_value":204,"agc":1,"agc_gain":0,"gainceiling":0,"bpc":0,"wpc":1,"raw_gma":1,"lenc":1,"vflip":1,"hmirror":1,"dcw":1,"colorbar":0,"cam_name":"ESP32 test camera","code_ver":"Mar 10 2022 @ 14:00:45","rotate":"0","stream_url":"http://10.0.0.181:81/"}
Reboot the camera
http://<IP-ADDRESS>/control?var=reboot&val=0

You can try these yourself in a browser address bar, from the commandline with curl and co. or use 
them programatically from your scripting language of choice.

