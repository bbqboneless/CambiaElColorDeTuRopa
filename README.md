# CambiaElColorDeTuRopa
Código con OpenCV para cambiar colores específicos de una imagen a partir del HSV.

Paso 1: Leer imagen de la computadora en BGR y convertir a su inverso para verlo en la escala normal de colores (RGB). Después cambiar al formato por tono, saturación y brillo (HSV). La imagen se va a trabajar a partir del formato HSV.

    img = cv2.imread('IMG-3568.jpg')
    img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

Paso 2: Crear una máscara que marque los límites del tono del color que vamos a cambiar (azul). En este caso el rango menor del azul en HSV es 90, 50 y 50, respectivamente; y el rango mayor del azul es de 130, 255, 255, respectivamente.

    lower = np.array([90, 50, 50])
    upper = np.array([130, 255, 255])
    mask_range = cv2.inRange(hsv, lower, upper)
    img_mask = cv2.bitwise_and(img, img, mask=mask_range)
    
Paso 3: Crear una nueva máscara que nos va a ayudar a cambiar sólo al color nuevo. Para esto solo vamos a cambiar el tono de nuestra imagen (-60) y aplicar la máscara para los rangos correspondientes a la máscara de la imagen original.

    img_hsv = cv2.cvtColor(img, cv2.COLOR_RGB2HSV)
    hue = img_hsv[:,:,0]
    hue = hue - 60
    img_hsv[:,:,0] = hue
    img_hsv_mask = cv2.bitwise_and(img_hsv, img_hsv, mask=mask_range)
    im = cv2.cvtColor(img_hsv_mask, cv2.COLOR_HSV2RGB)

Paso 4: Para obtener nuestra imagen cambiada, lo que hacemos simplemente es sumar la máscara nueva y la imagen original que corresponda a todo lo que no abarca la máscara.

    not_img_mask = cv2.bitwise_and(img, img, mask=~mask_range)
    img_final_final = not_img_mask + im
