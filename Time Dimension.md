//Working Hours 9:00am to 5:00pm
VAR Opening = 9
VAR Closing = 17

//Time Slot
VAR TimeSlot = 30
VAR Offset = (TimeSlot / 2) - 1

RETURN
ADDCOLUMNS (
    GENERATESERIES ( 0, 1439, 1 ), // 24 hours/day * 60 minutes/hour = 1,440 - 1 (initial time 12am)
    "Hour", HOUR( TIME ( 0, [Value], 0 )),
    "Minute", MINUTE( TIME ( 0, [Value], 0 )), 
    "Hour Minute 24h", FORMAT( TIME ( 0, [Value], 0 ),"hh:mm"),
    "Hour Minute 12h", FORMAT( TIME ( 0, [Value], 0 ),"hh:mm AM/PM"),
    "30 Minute Slot", FORMAT (TIME ( 0, MROUND(([Value] + Offset), TimeSlot), 0 ), "hh:mm"), // MROUND: Rounds Value to the nearest multiple of TimeSlot
    "Working Hours", IF(([Value] / 60) >= Opening && ([Value] / 60) <= Closing,"Yes","No") 
)
