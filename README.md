# No1) Vehicle Detection and Counting (OpenCV).


***This project detects moving vehicles in a video and counts how many cross a horizontal counting line.
It also separates counts into Left → Right and Right → Left.***


**Features**


*Background subtraction (MOG2)
Vehicle detection using contours
Counting line crossing detection
Direction-based counting
Live bounding boxes + center points
Final report after video ends*


***How It Works***


*Load video using OpenCV
Apply background subtraction
Detect contours (moving objects)
Filter by area (ignore noise)
Track object center
Count when crossing pink line
Split count by direction*


***Code For Example:***


**import cv2
import numpy as np**


**Video**


**cap = cv2.VideoCapture(r"A:\computer_Vision\2009.mp4")**


**Frame size**


**new_width, new_height = 600, 500**


**Lines position**


**pink_line_y = 250**


**Counters**


*left_count = 0
right_count = 0
crossed_pink = set()
frame_num = 0*


***Background subtractor***


*fgbg = cv2.createBackgroundSubtractorMOG2(
    history=500,
    varThreshold=50,
    detectShadows=True
)
print("Processing... Press 'n' to stop")*


***while cap.isOpened():***


   *ret, frame = cap.read()
    if not ret:
        break   
    frame = cv2.resize(frame, (new_width, new_height))
    frame_num += 1
    fgmask = fgbg.apply(frame)
    fgmask = cv2.medianBlur(fgmask, 5)
    contours, _ = cv2.findContours(
        fgmask,
        cv2.RETR_EXTERNAL,
        cv2.CHAIN_APPROX_SIMPLE
    )
    pink_color_bgr = (251, 0, 133)*
  
    
  ***Pink line***

    
  *cv2.line(frame, (0, pink_line_y),
             (new_width, pink_line_y),
             pink_color_bgr, 2)
    cv2.putText(frame,
                "COUNTING LINE",
                (10, pink_line_y - 5),
                cv2.FONT_HERSHEY_SIMPLEX,
                0.4,
                pink_color_bgr,1)*

                
***for contour in contours:***


*area = cv2.contourArea(contour)
        if 800 < area < 8000:
            x, y, w, h = cv2.boundingRect(contour)
            center_x = x + w // 2
            center_y = y + h // 2
            car_id = f"car_{center_x}_{center_y}"
            cv2.rectangle(frame,
                          (x, y),
                          (x+w, y+h),
                          (0, 255, 0),
                          2)
            cv2.circle(frame,
                       (center_x, center_y),
                       3,
                       (0, 0, 255),-1)*

                       
***Check crossing***


*if abs(center_y - pink_line_y) < 10:
     if car_id not in crossed_pink:
          crossed_pink.add(car_id)
              if center_x < new_width // 2:
                        left_count += 1
                    else:
                        right_count += 1
                    cv2.circle(frame,
                               (center_x, center_y),
                               12,
                               pink_color_bgr,
                               2)
                    cv2.putText(frame,
                                "COUNTED",
                                (center_x - 30, center_y - 15),
                                cv2.FONT_HERSHEY_SIMPLEX,
                                0.4,
                                pink_color_bgr,1)*

                                
**Display counts**


   *cv2.rectangle(frame, (5, 5), (200, 80), (0, 0, 0), -1)
    cv2.rectangle(frame, (5, 5), (200, 80), pink_color_bgr, 1)
    cv2.putText(frame,
                f"LEFT -> RIGHT: {left_count}",
                (10, 30),
                cv2.FONT_HERSHEY_SIMPLEX,
                0.5,
                (0, 255, 255),
                1)
    cv2.putText(frame,
                f"RIGHT -> LEFT: {right_count}",
                (10, 55),
                cv2.FONT_HERSHEY_SIMPLEX,
                0.5,
                (255, 255, 0),
                1)
    cv2.putText(frame,
                f"TOTAL: {left_count + right_count}",
                (10, 80),
                cv2.FONT_HERSHEY_SIMPLEX,
                0.5,
                pink_color_bgr,
                1)
    cv2.imshow("Car Detection", frame)
    if cv2.waitKey(1) & 0xFF == ord('n'):
        break
cap.release()
cv2.destroyAllWindows()*


****print("\n" + "="*40
print("FINAL REPORT")
print("="*40)
print(f"Left to Right: {left_count}")
print(f"Right to Left: {right_count}")
print(f"Total Vehicles: {left_count + right_count}")
print("="*40)*****


***How to Run***



***Output***


*Bounding box around vehicles
Pink counting line
Direction based counting
Final console report*


***Example Logic***


*LEFT SIDE   |   RIGHT SIDE*


------------|--------------

     ←      |      →
     ←      |      →

     
**Vehicles crossing the line are counted once only.**





# THIS IS THE FULL CODE:


```Python Code:

#                       ---DETECTING AND COUNTING---



# import cv2
# import numpy as np

# # Video
# cap = cv2.VideoCapture(r"A:\computer_Vision\2009.mp4")

# # Frame size
# new_width, new_height = 600, 500

# # Lines position
# pink_line_y = 250

# # Counters
# left_count = 0
# right_count = 0
# crossed_pink = set()  # Ab yeh permanently store hoga

# frame_num = 0

# # Background subtractor
# fgbg = cv2.createBackgroundSubtractorMOG2(history=500, varThreshold=50, detectShadows=True)

# print("Processing... Press 'n' to stop")

# while cap.isOpened():
#     ret, frame = cap.read()
#     if not ret:
#         break
    
#     frame = cv2.resize(frame, (new_width, new_height))
#     frame_num += 1
    
#     fgmask = fgbg.apply(frame)
#     fgmask = cv2.medianBlur(fgmask, 5)
#     contours, _ = cv2.findContours(fgmask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    
#     pink_color_bgr = (251, 0, 133)
    
#     # Pink line
#     cv2.line(frame, (0, pink_line_y), (new_width, pink_line_y), pink_color_bgr, 2)
#     cv2.putText(frame, "COUNTING LINE", (10, pink_line_y - 5), cv2.FONT_HERSHEY_SIMPLEX, 0.4, pink_color_bgr, 1)
    
#     for contour in contours:
#         area = cv2.contourArea(contour)
        
#         if 800 < area < 8000:
#             x, y, w, h = cv2.boundingRect(contour)
#             center_x = x + w // 2
#             center_y = y + h // 2
            
#             # Permanent unique ID based on position
#             car_id = f"car_{center_x}_{center_y}"
            
#             cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
#             cv2.circle(frame, (center_x, center_y), 3, (0, 0, 255), -1)
            
#             # Check if car crosses pink line
#             if abs(center_y - pink_line_y) < 10:
#                 if car_id not in crossed_pink:
#                     crossed_pink.add(car_id)
                    
#                     if center_x < new_width // 2:
#                         left_count += 1
#                     else:
#                         right_count += 1
                    
#                     cv2.circle(frame, (center_x, center_y), 12, pink_color_bgr, 2)
#                     cv2.putText(frame, "COUNTED", (center_x - 30, center_y - 15), 
#                                cv2.FONT_HERSHEY_SIMPLEX, 0.4, pink_color_bgr, 1)
    
#     # Display counts
#     cv2.rectangle(frame, (5, 5), (200, 80), (0, 0, 0), -1)
#     cv2.rectangle(frame, (5, 5), (200, 80), pink_color_bgr, 1)
    
#     cv2.putText(frame, f"LEFT -> RIGHT: {left_count}", (10, 30), 
#                cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 255), 1)
#     cv2.putText(frame, f"RIGHT -> LEFT: {right_count}", (10, 55), 
#                cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 255, 0), 1)
#     cv2.putText(frame, f"TOTAL: {left_count + right_count}", (10, 80), 
#                cv2.FONT_HERSHEY_SIMPLEX, 0.5, pink_color_bgr, 1)
    
#     cv2.putText(frame, "Press 'n' to stop", (new_width - 120, new_height - 10), 
#                cv2.FONT_HERSHEY_SIMPLEX, 0.4, (200, 200, 200), 1)
    
#     cv2.imshow("Car Detection", frame)
    
#     if cv2.waitKey(1) & 0xFF == ord('n'):
#         break

# cap.release()
# cv2.destroyAllWindows()

# print("\n" + "="*40)
# print("FINAL REPORT")
# print("="*40)
# print(f"Left to Right: {left_count}")
# print(f"Right to Left: {right_count}")
# print(f"Total Vehicles: {left_count + right_count}")
# print("="*40)
```





# No2) Vehicle Counting and Detecting (Part 2 - YOLOv8).


*This project detects and counts vehicles using YOLOv8.
Vehicles are counted when they cross a horizontal line.*
    

***Features***


*YOLOv8 vehicle detection
Line-crossing counting
Unique car ID tracking
Count only once per vehicle
Live bounding boxes
Total count display*


***Code For Example:***


*import cv2
from ultralytics import YOLO
model = YOLO('yolov8n.pt')
cap = cv2.VideoCapture(r"A:\computer_Vision\2011.mp4")
LINE_Y = 254
counted = set()              `counted cars`
current_crossing = set()*   `currently crossing cars`


***while cap.isOpened():***


  *ret, frame = cap.read()
    if not ret:
        break  
  frame = cv2.resize(frame, (500, 500))
    results = model(frame, conf=0.3)
    current_frame_cars = set()*

    
  ***if results[0].boxes is not None:***
        
        
  *for box in results[0].boxes.xywh.cpu().numpy():
            x, y, w, h = box
            x1, y1 = int(x-w/2), int(y-h/2)
            x2, y2 = int(x+w/2), int(y+h/2)*

            
***Stable ID:***


*car_id = f"{int(x/20)}_{int(y/20)}"
current_frame_cars.add(car_id)*


***# Line crossing check:***


*if abs(y - LINE_Y) < 15:
  if car_id not in counted:
  counted.add(car_id)
  current_crossing.add(car_id)
else:
      if car_id in current_crossing:
                    current_crossing.remove(car_id)*
                    
            
***Color selection:***


*if car_id in counted:
color = (0, 0, 255)  # red counted
else:
color = (0, 255, 0)  # green not counted          
cv2.rectangle(frame, (x1, y1), (x2, y2), color, 2)
cv2.putText(frame, str(car_id), (x1, y1-5),
cv2.FONT_HERSHEY_SIMPLEX, 0.4,
(255,255,255), 1)*

    
***remove disappeared cars:***


*for c in list(current_crossing):
        if c not in current_frame_cars:
            current_crossing.remove(c)  
  cv2.line(frame, (0, LINE_Y), (500, LINE_Y), (0, 255, 0), 2)
  cv2.putText(frame,
              f"COUNT: {len(counted)}",
                (10, 30),
                cv2.FONT_HERSHEY_SIMPLEX,
                0.8,
                (0,255,0),
                2)
    cv2.imshow("Car Counting", frame)
        if cv2.waitKey(1) & 0xFF == ord('n'):
        break
cap.release()
cv2.destroyAllWindows()*


**print(f"TOTAL: {len(counted)}")**



# THIS IS THE FULL CODE:


```Python Code:
#                 ---COUNTING AND DECTECTING(PRT2)---



# import cv2
# from ultralytics import YOLO

# model = YOLO('yolov8n.pt')
# cap = cv2.VideoCapture(r"A:\computer_Vision\2011.mp4")

# LINE_Y = 254
# counted = set()  # Jo cars count ho chuki hain
# current_crossing = set()  # Abhi line cross kar rahi hain

# while cap.isOpened():
#     ret, frame = cap.read()
#     if not ret:
#         break
    
#     frame = cv2.resize(frame, (500, 500))
#     results = model(frame, conf=0.3)
    
#     current_frame_cars = set()
    
#     if results[0].boxes is not None:
#         for box in results[0].boxes.xywh.cpu().numpy():
#             x, y, w, h = box
#             x1, y1 = int(x-w/2), int(y-h/2)
#             x2, y2 = int(x+w/2), int(y+h/2)
            
#             # Stable ID
#             car_id = f"{int(x/20)}_{int(y/20)}"
#             current_frame_cars.add(car_id)
            
#             # Line cross check - sirf ek baar count
#             if abs(y - LINE_Y) < 15:
#                 if car_id not in counted:
#                     counted.add(car_id)
#                     current_crossing.add(car_id)
#             else:
#                 if car_id in current_crossing:
#                     current_crossing.remove(car_id)
            
#             # Color
#             if car_id in counted:
#                 color = (0, 0, 255)  # Red - count ho chuki
#             else:
#                 color = (0, 255, 0)  # Green - abhi nahi hui
            
#             cv2.rectangle(frame, (x1, y1), (x2, y2), color, 2)
#             cv2.putText(frame, str(car_id), (x1, y1-5), cv2.FONT_HERSHEY_SIMPLEX, 0.4, (255,255,255), 1)
    
#     # Clean up - jo cars gayab ho gayi unhe current_crossing se hatao
#     for c in list(current_crossing):
#         if c not in current_frame_cars:
#             current_crossing.remove(c)
    
#     cv2.line(frame, (0, LINE_Y), (500, LINE_Y), (0, 255, 0), 2)
#     cv2.putText(frame, f"COUNT: {len(counted)}", (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 0.8, (0,255,0), 2)
#     cv2.imshow("Car Counting", frame)
    
#     if cv2.waitKey(1) & 0xFF == ord('n'):
#         break

# cap.release()
# cv2.destroyAllWindows()
# print(f"TOTAL: {len(counted)}")
```







# No3) Tracking and Detecting with Two Lines (YOLOv8).

***This project tracks vehicles using YOLOv8 and counts them using two horizontal lines:***


*Red line → Going Down*
*Blue line → Going Up*


**Each vehicle is assigned a unique ID and counted once.**


***Features***


*YOLOv8 detection
Vehicle ID tracking
Two-line counting system
Up / Down direction detection
Bounding boxes + IDs
Final summary output*


***Code For Example:***


*import cv2
import pandas as pd
from ultralytics import YOLO
import math
model = YOLO('yolov8n.pt')
cap = cv2.VideoCapture(r"A:\computer_Vision\2011.mp4")*


***Store car positions for tracking***


*car_tracker = {}
next_id = 0*


***Counted cars:***


*down_counted = set()
up_counted = set()
frame_count = 0
while True:
    ret, frame = cap.read()
    if not ret:
        break
    frame_count += 1
    if frame_count % 2 != 0:
        continue
    frame = cv2.resize(frame, (1020, 500))
    results = model(frame, conf=0.3, verbose=False)
    red_line_y = 198
    blue_line_y = 268
    current_cars = []  
    if results[0].boxes is not None:
        for box in results[0].boxes.xywh.cpu().numpy():
            x, y, w, h = box
            x1, y1 = int(x - w/2), int(y - h/2)
            x2, y2 = int(x + w/2), int(y + h/2)
            cx = int(x)
            cy = int(y)*
            
            
***Find existing car or assign new ID***


*car_id = None
for cid, (px, py) in car_tracker.items():
if math.hypot(cx - px, cy - py) < 50:
  car_id = cid
      break*
            
  ***if car_id is None:***

  
*car_id = next_id
  next_id += 1
  car_tracker[car_id] = (cx, cy)
  current_cars.append(car_id)*
  
            
***Check line crossing:***


  *if abs(cy - red_line_y) < 10:
    if car_id not in down_counted and car_id not in up_counted:
      down_counted.add(car_id)
                    cv2.circle(frame, (cx, cy), 5, (0, 0, 255), -1)
                    cv2.putText(frame, f"DOWN:{car_id}",
                                (cx, cy-10),
                                cv2.FONT_HERSHEY_SIMPLEX,
                                0.5,
                                (0,255,255),
                                1)
                        if abs(cy - blue_line_y) < 10:
                            if car_id not in up_counted and car_id not in down_counted:
                                  up_counted.add(car_id)
                                      cv2.circle(frame, (cx, cy), 5, (0, 0, 255), -1)
                                      cv2.putText(frame, f"UP:{car_id}",
                                      (cx, cy-10),
                                      cv2.FONT_HERSHEY_SIMPLEX,
                                      0.5,
                                      (0,255,255),1)*
            
***Draw rectangle:***


*cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0), 2)
            cv2.putText(frame, str(car_id),
                        (x1, y1-5),
                        cv2.FONT_HERSHEY_SIMPLEX,
                        0.4,
                        (255,255,255),1)*
                        
    
***Remove old cars:***


*for cid in list(car_tracker.keys()):
        if cid not in current_cars:
            del car_tracker[cid]*

    
  ***Draw lines:***

  
  *cv2.line(frame, (0, red_line_y),
             (frame.shape[1], red_line_y),
             (0, 0, 255), 2)
    cv2.line(frame, (0, blue_line_y),
             (frame.shape[1], blue_line_y),
             (255, 0, 0), 2)  
    cv2.putText(frame, "RED LINE",
                (10, red_line_y-5),
                cv2.FONT_HERSHEY_SIMPLEX,0.5,
                (255,255,255),1)
    cv2.putText(frame, "BLUE LINE",
                (10, blue_line_y-5),
                cv2.FONT_HERSHEY_SIMPLEX,0.5,
                (255,255,255),1)
        cv2.putText(frame,
                f"GOING DOWN: {len(down_counted)}",
                (20, 40),
                cv2.FONT_HERSHEY_SIMPLEX,
                0.7,
                (0,255,0),2)
    cv2.putText(frame,
                f"GOING UP: {len(up_counted)}",
                (20, 70),
                cv2.FONT_HERSHEY_SIMPLEX,
                0.7,
                (0,255,255),2)
    cv2.imshow("Vehicle Counting", frame)
    if cv2.waitKey(1) & 0xFF == 110:
        break
cap.release()
cv2.destroyAllWindows()*


**print(f"\n=== FINAL ===")
print(f"Going Down: {len(down_counted)}")
print(f"Going Up: {len(up_counted)}")
print(f"Total: {len(down_counted) + len(up_counted)}")**




# THIS IS THE FULL CODE:


```Python Code:


#               ---TRACKING AND DETECTING WITH 2 LINES---



# import cv2
# import pandas as pd
# from ultralytics import YOLO
# import math

# model = YOLO('yolov8n.pt')
# cap = cv2.VideoCapture(r"A:\computer_Vision\2011.mp4")

# # Store car positions for tracking
# car_tracker = {}
# next_id = 0

# Counted cars.

# down_counted = set()
# up_counted = set()

# frame_count = 0

# while True:
#     ret, frame = cap.read()
#     if not ret:
#         break
    
#     frame_count += 1
#     if frame_count % 2 != 0:
#         continue
    
#     frame = cv2.resize(frame, (1020, 500))
#     results = model(frame, conf=0.3, verbose=False)
    
#     red_line_y = 198
#     blue_line_y = 268
#     current_cars = []
    
#     if results[0].boxes is not None:
#         for box in results[0].boxes.xywh.cpu().numpy():
#             x, y, w, h = box
#             x1, y1 = int(x - w/2), int(y - h/2)
#             x2, y2 = int(x + w/2), int(y + h/2)
#             cx = int(x)
#             cy = int(y)
            
#             # Find existing car or assign new ID
#             car_id = None
#             for cid, (px, py) in car_tracker.items():
#                 if math.hypot(cx - px, cy - py) < 50:
#                     car_id = cid
#                     break
            
#             if car_id is None:
#                 car_id = next_id
#                 next_id += 1
            
#             car_tracker[car_id] = (cx, cy)
#             current_cars.append(car_id)
            
#             # Check line crossing
#             if abs(cy - red_line_y) < 10:
#                 if car_id not in down_counted and car_id not in up_counted:
#                     down_counted.add(car_id)
#                     cv2.circle(frame, (cx, cy), 5, (0, 0, 255), -1)
#                     cv2.putText(frame, f"DOWN:{car_id}", (cx, cy-10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0,255,255), 1)
            
#             if abs(cy - blue_line_y) < 10:
#                 if car_id not in up_counted and car_id not in down_counted:
#                     up_counted.add(car_id)
#                     cv2.circle(frame, (cx, cy), 5, (0, 0, 255), -1)
#                     cv2.putText(frame, f"UP:{car_id}", (cx, cy-10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0,255,255), 1)
            
#             # Draw rectangle
#             cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0), 2)
#             cv2.putText(frame, str(car_id), (x1, y1-5), cv2.FONT_HERSHEY_SIMPLEX, 0.4, (255,255,255), 1)
    
#     # Remove old cars
#     for cid in list(car_tracker.keys()):
#         if cid not in current_cars:
#             del car_tracker[cid]
    
#     # Draw lines
#     cv2.line(frame, (0, red_line_y), (frame.shape[1], red_line_y), (0, 0, 255), 2)
#     cv2.line(frame, (0, blue_line_y), (frame.shape[1], blue_line_y), (255, 0, 0), 2)
    
#     cv2.putText(frame, f"RED LINE", (10, red_line_y-5), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255,255,255), 1)
#     cv2.putText(frame, f"BLUE LINE", (10, blue_line_y-5), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255,255,255), 1)
    
#     cv2.putText(frame, f"GOING DOWN: {len(down_counted)}", (20, 40), cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0,255,0), 2)
#     cv2.putText(frame, f"GOING UP: {len(up_counted)}", (20, 70), cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0,255,255), 2)
    
#     cv2.imshow("Vehicle Counting", frame)
    
#     if cv2.waitKey(1) & 0xFF == 110:
#         break

# cap.release()
# cv2.destroyAllWindows()
# print(f"\n=== FINAL ===")
# print(f"Going Down: {len(down_counted)}")
# print(f"Going Up: {len(up_counted)}")
# print(f"Total: {len(down_counted) + len(up_counted)}")
```












# No4) Vehicle Tracking Only (YOLOv8 + Custom Tracker).


***This project tracks vehicles using YOLOv8 and assigns a unique ID to each object.
It only performs tracking, no counting.***


**Features:**


*YOLOv8 object detection
Custom centroid tracker
Unique ID for each vehicle
Bounding boxes with IDs
Real-time tracking*


***Code For Exapmle:***


*import cv2
import math
from ultralytics import YOLO*


***TRACKER CLASS:***


*class Tracker:
    def __init__(self):
        self.center_points = {}
        self.id_count = 0
    def update(self, objects_rect):
        objects_bbs_ids = []
        for rect in objects_rect:
            x, y, w, h = rect
            cx = (x + x + w) // 2
            cy = (y + y + h) // 2
            same_object_detected = False
            for obj_id, pt in self.center_points.items():
                dist = math.hypot(cx - pt[0], cy - pt[1])
                if dist < 35:
                    self.center_points[obj_id] = (cx, cy)
                    objects_bbs_ids.append([x, y, w, h, obj_id])
                    same_object_detected = True
                    break
            if not same_object_detected:
                self.center_points[self.id_count] = (cx, cy)
                objects_bbs_ids.append([x, y, w, h, self.id_count])
                self.id_count += 1
        new_center_points = {}
        for obj_bb_id in objects_bbs_ids:
            _, _, _, _, object_id = obj_bb_id
            if object_id in self.center_points:
                new_center_points[object_id] = self.center_points[object_id]
        self.center_points = new_center_points.copy()
        return objects_bbs_ids*

        
***MAIN CODE:***


*model = YOLO('yolov8n.pt')
cap = cv2.VideoCapture(r"A:\computer_Vision\2011.mp4")
tracker = Tracker()
while True:
    ret, frame = cap.read()
    if not ret:
        break
    frame = cv2.resize(frame, (800, 500))
    results = model(frame, conf=0.3, verbose=False)
    car_boxes = []
    if results[0].boxes is not None:
        for box in results[0].boxes.xywh.cpu().numpy():
            x, y, w, h = box
            x1 = int(x - w/2)
            y1 = int(y - h/2)
            w = int(w)
            h = int(h)
            car_boxes.append([x1, y1, w, h])*


***Get tracking IDs:***


*tracked_objects = tracker.update(car_boxes)
    for obj in tracked_objects:
        x, y, w, h, obj_id = obj
        cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
        cv2.putText(frame,
                    f"ID:{obj_id}",
                    (x, y-5),
                    cv2.FONT_HERSHEY_SIMPLEX,
                    0.5,
                    (255,255,255),
                    1)
    cv2.imshow("Tracking", frame)
    if cv2.waitKey(1) & 0xFF == ord('n'):
        break
cap.release()
cv2.destroyAllWindows()*



# THIS IS THE FULL CODE:


```Python Code:

#                   ---ONLY FOR TRACKING---



# import cv2
# import math
# from ultralytics import YOLO

# TRACKER CLASS.


# class Tracker:
#     def __init__(self):
#         self.center_points = {}
#         self.id_count = 0

#     def update(self, objects_rect):
#         objects_bbs_ids = []

#         for rect in objects_rect:
#             x, y, w, h = rect
#             cx = (x + x + w) // 2
#             cy = (y + y + h) // 2

#             same_object_detected = False
#             for obj_id, pt in self.center_points.items():
#                 dist = math.hypot(cx - pt[0], cy - pt[1])

#                 if dist < 35:
#                     self.center_points[obj_id] = (cx, cy)
#                     objects_bbs_ids.append([x, y, w, h, obj_id])
#                     same_object_detected = True
#                     break

#             if not same_object_detected:
#                 self.center_points[self.id_count] = (cx, cy)
#                 objects_bbs_ids.append([x, y, w, h, self.id_count])
#                 self.id_count += 1

#         new_center_points = {}
#         for obj_bb_id in objects_bbs_ids:
#             _, _, _, _, object_id = obj_bb_id
#             if object_id in self.center_points:
#                 new_center_points[object_id] = self.center_points[object_id]

#         self.center_points = new_center_points.copy()
#         return objects_bbs_ids


# MAIN CODE.


# model = YOLO('yolov8n.pt')
# cap = cv2.VideoCapture(r"A:\computer_Vision\2011.mp4")  # Apni video ka path do

# tracker = Tracker()

# while True:
#     ret, frame = cap.read()
#     if not ret:
#         break
    
#     frame = cv2.resize(frame, (800, 500))
#     results = model(frame, conf=0.3, verbose=False)
    
#     car_boxes = []
    
#     if results[0].boxes is not None:
#         for box in results[0].boxes.xywh.cpu().numpy():
#             x, y, w, h = box
#             x1 = int(x - w/2)
#             y1 = int(y - h/2)
#             w = int(w)
#             h = int(h)
#             car_boxes.append([x1, y1, w, h])
    
#     # Tracker se IDs lelo
#     tracked_objects = tracker.update(car_boxes)
    
#     for obj in tracked_objects:
#         x, y, w, h, obj_id = obj
#         cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
#         cv2.putText(frame, f"ID:{obj_id}", (x, y-5), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255,255,255), 1)
    
#     cv2.imshow("Tracking", frame)
    
#     if cv2.waitKey(1) & 0xFF == ord('n'):
#         break

# cap.release()
# cv2.destroyAllWindows()
```








