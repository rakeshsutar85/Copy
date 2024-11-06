# Copy


text/x-generic app.py ( Python script, ASCII text executable )
from flask import Flask,request,json,jsonify
import cv2
import numpy as np
import utlis
import os 
import urllib.request
from werkzeug.utils import secure_filename

app = Flask(__name__)

UPLOAD_FOLDER = 'sheet/'
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER
app.config['MAX_CONTENT_LENGTH'] = 16 * 1024 * 1024
 
ALLOWED_EXTENSIONS = set(['txt', 'pdf', 'png', 'jpg', 'jpeg', 'gif'])

@app.route('/<pathname>')
def hello(pathname):

    value = processOMR(pathname)
    return value


    
def processOMR(pathname):
    widthImg = 700
    heightImg = 700
    
    img = cv2.imread('sheet/'+pathname)
    img = cv2.resize(img,(widthImg,heightImg))
    
    # FIND Regd
    y=70
    x=110
    h=210
    w=235
    
    crop_image = img[x:w, y:h]
    
    imgwarpGrayregd = cv2.cvtColor(crop_image,cv2.COLOR_BGR2GRAY)
    imgthresregd = cv2.threshold(imgwarpGrayregd,140,255,cv2.THRESH_BINARY_INV)[1]
    height, width = imgthresregd.shape
    W_SIZE  = 7 
    H_SIZE = 10
    myPixelVal = np.zeros((7,10))
    countR=0
    countC=0
    roll_no = [0,0,0,0,0,0,0]  
    
    for ih in range(H_SIZE ):
          for iw in range(W_SIZE ):
              x = width/W_SIZE * iw
              y = height/H_SIZE * ih
              h = (height / H_SIZE)
              w = (width / W_SIZE )
              img = imgthresregd[int(y):int(y+h), int(x):int(x+w)]
              totalPixels = cv2.countNonZero(img)
              myPixelVal[countR][countC]= totalPixels            
              if totalPixels > 50 and ih == 0  :
                        roll_no[iw] = ih
              if totalPixels > 50 and ih == 1  :
                        roll_no[iw] = ih
              if totalPixels > 50 and ih == 2  :
                        roll_no[iw] = ih
              if totalPixels > 50 and ih == 3  :
                        roll_no[iw] = ih
              if totalPixels > 50 and ih == 4  :
                        roll_no[iw] = ih
              if totalPixels > 50 and ih == 5  :
                        roll_no[iw] = ih
              if totalPixels > 50 and ih == 6  :
                        roll_no[iw] = ih
              if totalPixels > 50 and ih == 7  :
                        roll_no[iw] = ih
              if totalPixels > 50 and ih == 8  :
                        roll_no[iw] = ih
              if totalPixels > 50 and ih == 9  :
                        roll_no[iw] = ih

    data = dict()
    data['roll_no'] = roll_no
    #Roll No Detection End
    
    y=70
    x=270
    h=150
    w=520
    
    img = cv2.imread('sheet/'+pathname)
    img = cv2.resize(img,(widthImg,heightImg))
    crop_image = img[x:w, y:h]
    
    imgGray120 = cv2.cvtColor(crop_image,cv2.COLOR_BGR2GRAY)
    imgthres120= cv2.threshold(imgGray120,140,255,cv2.THRESH_BINARY_INV)[1]

    height, width = imgthres120.shape
    W_SIZE  = 4
    H_SIZE = 20
    myPixelVal = np.zeros((7,10))
    countR=0
    countC=0
    atemptQ1 = []  
    atempt = ["null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null"]
    for ih in range(H_SIZE ):
        for iw in range(W_SIZE ):
                x = width/W_SIZE * iw 
                y = height/H_SIZE * ih
                h = (height / H_SIZE)
                w = (width / W_SIZE )
                     
                img = imgthres120[int(y):int(y+h), int(x):int(x+w)]
                totalPixels = cv2.countNonZero(img)
                myPixelVal[countR][countC]= totalPixels
                atemptQ1.append(totalPixels)
    
    array = np.array_split(atemptQ1, 20)
    for  ia in range(0, len(array)):
        eacharr = array[ia]
        pos = eacharr.argmax()
        if(pos == 0):
         atempt[ia] = 'A'
        if(pos == 1):
         atempt[ia] = 'B'
        if(pos == 2):
         atempt[ia] = 'C'
        if(pos == 3):
         atempt[ia] = 'D'
    # 1- 20 Detection End
    y=190
    x=270
    h=270
    w=520
    img = cv2.imread('sheet/'+pathname)
    img = cv2.resize(img,(widthImg,heightImg))
    
    crop_image2140 = img[x:w, y:h]


    imgGray2140 = cv2.cvtColor(crop_image2140,cv2.COLOR_BGR2GRAY)
    imgthres2140 = cv2.threshold(imgGray2140,140,255,cv2.THRESH_BINARY_INV)[1]
    height, width = imgthres2140.shape\

    W_SIZE  = 4
    H_SIZE = 20
    myPixelVal = np.zeros((7,10))
    countR=0
    countC=0
    atemptQ2 = []
    atempt2 = ["null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null"]
    for ih in range(H_SIZE ):
     for iw in range(W_SIZE ):
        x = width/W_SIZE * iw
        y = height/H_SIZE * ih
        h = (height / H_SIZE)
        w = (width / W_SIZE )
        img = imgthres2140[int(y):int(y+h), int(x):int(x+w)]
        totalPixels = cv2.countNonZero(img)
        myPixelVal[countR][countC]= totalPixels 
        atemptQ2.append(totalPixels) 
    
    array = np.array_split(atemptQ2, 20) 
    for  ia in range(0, len(array)):
        eacharr = array[ia]
        pos = eacharr.argmax()
        if(pos == 0):
         atempt2[ia] = 'A'
        if(pos == 1):
         atempt2[ia] = 'B'
        if(pos == 2):
         atempt2[ia] = 'C'
        if(pos == 3):
         atempt2[ia] = 'D'
    # 21- 40 Detection End
    
    y=310
    x=270
    h=400
    w=520
    
    img = cv2.imread('sheet/'+pathname)
    img = cv2.resize(img,(widthImg,heightImg))

    crop_image4160 = img[x:w, y:h]

    imgGray4160 = cv2.cvtColor(crop_image4160,cv2.COLOR_BGR2GRAY)
    imgthres4160= cv2.threshold(imgGray4160,140,255,cv2.THRESH_BINARY_INV)[1]
    height, width = imgthres4160.shape

    W_SIZE  = 4
    H_SIZE = 20
    myPixelVal = np.zeros((7,10))
    countR=0
    countC=0
    atemptQ3 = []
    atempt3 = ["null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null"]
    
    for ih in range(H_SIZE ):
     for iw in range(W_SIZE ):
        x = width/W_SIZE * iw
        y = height/H_SIZE * ih
        h = (height / H_SIZE)
        w = (width / W_SIZE )
        img = imgthres4160[int(y):int(y+h), int(x):int(x+w)]
            
        totalPixels = cv2.countNonZero(img)
        myPixelVal[countR][countC]= totalPixels 
        atemptQ3.append(totalPixels) 
    
    array = np.array_split(atemptQ3, 20) 
    for  ia in range(0, len(array)):
         eacharr = array[ia]
         pos = eacharr.argmax()
         if(pos == 0 ):
          atempt3[ia] = 'A'
         if(pos == 1):
          atempt3[ia] = 'B'
         if(pos == 2):
          atempt3[ia] = 'C'
         if(pos == 3):
          atempt3[ia] = 'D'
    # 41- 60 Detection End
    
    y=440
    x=270
    h=530
    w=520
    
    img = cv2.imread('sheet/'+pathname)
    img = cv2.resize(img,(widthImg,heightImg))

    crop_image6180 = img[x:w, y:h]
    

    imgwarpGray6180 = cv2.cvtColor(crop_image6180,cv2.COLOR_BGR2GRAY)
    imgthres6180= cv2.threshold(imgwarpGray6180,140,255,cv2.THRESH_BINARY_INV)[1]
    height, width = imgthres6180.shape
    W_SIZE  = 4
    H_SIZE = 20
    myPixelVal = np.zeros((7,10))
    countR=0
    countC=0
    atemptQ4 = []
    atempt4 = ["null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null"]


    for ih in range(H_SIZE ):
     for iw in range(W_SIZE ):
        x = width/W_SIZE * iw
        y = height/H_SIZE * ih
        h = (height / H_SIZE)
        w = (width / W_SIZE )
        img = imgthres6180[int(y):int(y+h), int(x):int(x+w)]
            
        totalPixels = cv2.countNonZero(img)
        myPixelVal[countR][countC]= totalPixels 
        atemptQ4.append(totalPixels) 
    
    array = np.array_split(atemptQ4, 20) 
    for  ia in range(0, len(array)):
         eacharr = array[ia]
         pos = eacharr.argmax()
         if(pos == 0 ):
          atempt4[ia] = 'A'
         if(pos == 1):
          atempt4[ia] = 'B'
         if(pos == 2):
          atempt4[ia] = 'C'
         if(pos == 3):
          atempt4[ia] = 'D'
          
    # 61- 80 Detection End
    y=440
    x=270
    h=530
    w=520
    img = cv2.imread('sheet/'+pathname)
    img = cv2.resize(img,(widthImg,heightImg))

    crop_image81100 = img[x:w, y:h]
    

    imgwarpGray81100 = cv2.cvtColor(crop_image81100,cv2.COLOR_BGR2GRAY)
    imgthres81100= cv2.threshold(imgwarpGray81100,140,255,cv2.THRESH_BINARY_INV)[1]
    height, width = imgthres81100.shape
    W_SIZE  = 4
    H_SIZE = 20
    myPixelVal = np.zeros((7,10))
    countR=0
    countC=0
    atemptQ5 = []
    atempt5 = ["null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null","null"]


    for ih in range(H_SIZE ):
     for iw in range(W_SIZE ):
        x = width/W_SIZE * iw
        y = height/H_SIZE * ih
        h = (height / H_SIZE)
        w = (width / W_SIZE )
        img = imgthres81100[int(y):int(y+h), int(x):int(x+w)]
            
        totalPixels = cv2.countNonZero(img)
        myPixelVal[countR][countC]= totalPixels 
        atemptQ5.append(totalPixels) 
    
    array = np.array_split(atemptQ5, 20) 
    for  ia in range(0, len(array)):
         eacharr = array[ia]
         pos = eacharr.argmax()
         if(pos == 0 ):
          atempt5[ia] = 'A'
         if(pos == 1):
          atempt5[ia] = 'B'
         if(pos == 2):
          atempt5[ia] = 'C'
         if(pos == 3):
          atempt5[ia] = 'D'

    data['attempt'] = atempt + atempt2 + atempt3 + atempt4 + atempt5



    return data
    
@app.route('/upload', methods=['POST'])
def upload_file():
    if 'files' not in request.files:
        resp = jsonify({'message' : 'No file part in the request'})
        resp.status_code = 400
        return resp
    else:
        files = request.files.getlist('files')
     
        errors = {}
        success = False
        
        f = request.files['files']  
        f.save(app.config['UPLOAD_FOLDER'] + f.filename)  
        return 'YES'
    
    
 
        

  
    
    
    
    
  
