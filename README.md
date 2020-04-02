# mouni1610
import RPi.GPIO as GPIO
import serial
import time
import lcd
import wifi
import urllib2
GPIO.setmode(GPIO.BOARD)
GPIO.setwarnings(False)

URL = 'http://embsupermarket.dbandroid.online/bill.php?' 
lcd.lcd_init()
lcd.stringlcd(0x80,"WELCOME")
time.sleep(1)
globalc1=0
globalc2=0
globalc3=0
globalc4=0
globalamount=0
id=""

def  upload():
    print 'UPLOADING'
    lcd.stringlcd(0x80,"SENDING")
    finalURL = URL +"&id=%s" %(id)
    s=urllib2.urlopen(finalURL);
    s.close()
    time.sleep(1)
    lcd.stringlcd(0x80,"UPLOADED>>")
    time.sleep(1)
    lcd.stringlcd(0x80,"SMART SHOPPING")
    time.sleep(1)
    
 
    
    


wifi.Connect_wifi()
time.sleep(1)
lcd.stringlcd(0x80,"SMART SHOPPING")
time.sleep(1)



def read_rfid ():
   ser = serial.Serial ("/dev/ttyS0")                           #Open named port 
   ser.baudrate = 9600                                            #Set baud rate to 9600
   data = ser.read(12)                                            #Read 12 characters from serial port to data
   ser.close ()                                                   #Close port
   return data                                                    #Return data
#id = read_rfid ()                                              #Function call
#print id                                                       #Print RFID
GPIO.add_event_detect(12, GPIO.FALLING, callback=my_callback, bouncetime=300) 

while(1):
    id = read_rfid ()
    print id
    time.sleep(1)
    if(id=="3600A4847E68"):
        print "card1"
        if(globalc1==0):
            lcd.stringlcd(0x80,"PEN")
            globalamount=globalamount+10
            globalc1=1
            lcd.stringlcd(0xC0,str(globalamount))
            print "add"
            print  globalamount
            time.sleep(1)
            lcd.stringlcd(0x80,"PLACE ITEM")
            lcd.stringlcd(0xC0,"total:"+str(globalamount))
            upload()
            
        else:
            
            lcd.stringlcd(0x80,"PEN REM>>> ")
            globalamount=globalamount-10
            
            print "sub"
            print   globalamount
            globalc1=0
            time.sleep(1)
            lcd.stringlcd(0x80,"PLACE ITEM")
            lcd.stringlcd(0xC0,"total:"+str(globalamount))
            upload()
        
        
        

        id=""
    elif (id=="3600A5F7FA9E"):
        print "card 2"
        id=""
        
        if(globalc2==0):
            globalamount=globalamount+100
            globalc2=1
            lcd.stringlcd(0x80,"SOAP ")
            
            lcd.stringlcd(0xC0,"total:"+str(globalamount))
            print "add"
            print globalamount
            time.sleep(1)
            lcd.stringlcd(0x80,"PLACE ITEM")
            lcd.stringlcd(0xC0,"total:"+str(globalamount))
            upload()
            
        else:
            globalamount=globalamount-100
            lcd.stringlcd(0x80,"SOAP REM>>> ")
            
            lcd.stringlcd(0xC0,str(globalamount))
            print "sub"
            globalc2=0
            time.sleep(1)
            lcd.stringlcd(0x80,"PLACE ITEM")
            lcd.stringlcd(0xC0,"total:"+str(globalamount))
            upload()
    elif (id=="3600A511F476"):
        print "card 3"
        id=""
        if(globalc3==0):
            globalamount=globalamount+1000
            globalc3=1
            lcd.stringlcd(0x80,"RICE BAG ")
            
            lcd.stringlcd(0xC0,"total"+str(globalamount))
            print "add"
            print globalamount
            time.sleep(1)
            lcd.stringlcd(0x80,"PLACE ITEM")
            lcd.stringlcd(0xC0,"total:"+str(globalamount))
            upload()
            
        else:
            globalamount=globalamount-1000
            print "sub"
            lcd.stringlcd(0x80,"RICE BAG REM>>")
            
            lcd.stringlcd(0xC0,str(globalamount))
            print globalamount
            globalc3=0
            time.sleep(1)
            lcd.stringlcd(0x80,"PLACE ITEM")
            lcd.stringlcd(0xC0,"total:"+str(globalamount))
            upload()
    elif (id=="3600A4968D89"):
        print "card 4"
        id=""
        if(globalc4==0):
            globalamount=globalamount+50
            globalc4=1
            lcd.stringlcd(0x80,"choclet")
            
            lcd.stringlcd(0xC0,"total:"+str(globalamount))
            print globalamount
            print "add"
            time.sleep(1)
            lcd.stringlcd(0x80,"PLACE ITEM")
            lcd.stringlcd(0xC0,"total:"+str(globalamount))
            upload()
            
        else:
            globalamount=globalamount-50
            print globalamount
            lcd.stringlcd(0x80,"choclet rem >>>")
            
            lcd.stringlcd(0xC0,str(globalamount))
            print "sub"
            globalc4=0
            time.sleep(1)
            lcd.stringlcd(0x80,"PLACE ITEM")
            lcd.stringlcd(0xC0,"total:"+str(globalamount))
            upload()
        
