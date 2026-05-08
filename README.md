# Brazilian-drones
Drones be ballin' like never before!


https://app.clickup.com/90121681571/chat/r/7-90121681571-8 

https://miro.com/app/board/uXjVHb9N-U4=/?share_link_id=181904767848 




Problemstilling: Vi mennesker, er ikke spor høje, (undtagen mads han siger, han er 196) derfor har vi tænkt at vi vha. af vores drone skal kunne få "nogle"/noget ned fra et højt sted vha. vores drone som vi skal programere


Da vi har haft problemer med python (og dronen), har vi indtilvidre  lavet et simpelt flowchart der kan give os en ide til hvad vi skal arbejde hen imod når vi får python til at virke.


<img width="584" height="823" alt="image" src="https://github.com/user-attachments/assets/472f2d8d-327c-4a6b-ac13-aa372a3e85e8" />

Torsdag 7. Maj:
Idag fik vi endelig dronen til at fungere!
Idag fik vi dronen til at flyve op og ned samt dreje til siderne. Der er stadig få problemer med at få dronen til at dreje flyve frem og tilbage, men det skal vi få fixet. Either way kom vi langt idag! 

```Py
from djitellopy import Tello
import cv2

tello = Tello()
tello.connect()
print(f"Battery: {tello.get_battery()}%")

tello.streamon()
frame_reader = tello.get_frame_read()

flying = False
distance = 50  

try:
    while True:
        frame = frame_reader.frame
        if frame is not None:
            frame = cv2.resize(frame, (960, 720))
            cv2.putText(frame, f"Battery: {tello.get_battery()}%",
                        (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 0.8, (0,255,0), 2)
            cv2.putText(frame, f"Distance per key: {distance}cm",
                        (10, 60), cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255,255,0), 1)
            cv2.imshow("Tello Camera", frame)

        key = cv2.waitKey(50) & 0xFF

        if key == 27:    # ESC
            break
        elif key == ord(' ') and not flying:
            tello.takeoff()
            flying = True
        elif key == ord('l') and flying:
            tello.land()
            flying = False
        elif key == ord('e'):
            tello.emergency()

        elif key == 81 and flying:  tello.move_forward(distance)
        elif key == 84 and flying:  tello.move_back(distance)
        elif key == 82 and flying:  tello.move_left(distance)
        elif key == 83 and flying:  tello.move_right(distance)
        elif key == ord('w') and flying:  tello.move_up(distance)
        elif key == ord('s') and flying:  tello.move_down(distance)
        elif key == ord('a') and flying:  tello.rotate_counter_clockwise(45)
        elif key == ord('d') and flying:  tello.rotate_clockwise(45)

        
        elif key == ord('+'):  distance = min(distance + 10, 500)
        elif key == ord('-'):  distance = max(distance - 10, 20)

finally:
    if flying:
        tello.land()
    tello.streamoff()
    tello.end()
    cv2.destroyAllWindows()
```

### 3 Lags modellen:

Præsentationslag- I præsentationslagen har vi vores input som er "SPACEBAR" for at lette, "A og D" til at rotere, Pil tasterne for at ændre højden. Ift. Grafik er det meget blåt i kameraet, men man kan godt tage billeder fra computeren. Vores drone kan regne ud hvor langt den skal hen til dens punkt og hvis den ikke kan regne ud så stopper den bare med at fungere.

Logiklag- Den foretager logiske beregninger, ved at regne ud hvor langt den skal hen til dens punkt. Vores drone kan hente informationer og data fra præsentationslaget, den kan også hente fra selve datalaget som der laver bergeningerne.

Datalag- Vores datalag opbevarer data fra vores drone, og man kan også søge i alt vores data fordi den går igennem alt vores data. Den kan sende information til logiklaget så den ved hvilken data den skal bruge
