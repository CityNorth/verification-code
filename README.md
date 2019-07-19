# verification-code
验证码生成器 


后期会加入使用TensorFlow验证码识别


### 图形验证码的识别

1. 直接将图片识别

		import tesserocr
		from PIL import Image
		
		image = Image.open('code2.jpg')
		result = tesserocr.image_to_text(image)
		print(result)

2. 将图片进行简单处理后识别，正确率较高

		import tesserocr
		from PIL import Image
		
		image = Image.open('code2.jpg')
		#向量化处理图片  方便识别
		image = image.convert('L')
		#调整阈值
		threshold = 127
		table = []
		for i in range(256):
		    if i < threshold:
		        table.append(0)
		    else:
		        table.append(1)
		
		image = image.point(table, '1')
		image.show()
		
		result = tesserocr.image_to_text(image)
		print(result)

3. 示例操作

		import tesserocr
		from PIL import Image
		
		def get_image():
			#将网页截屏
		    browser.save_screenshot('jietu.png')
			#找到验证码位置
		    img=browser.find_element_by_id("chkimg")
			#获取验证码x,y轴坐标
		    location = img.location
		#     print(location)
			#获取验证码的长宽
		    size =img.size
			#写成我们需要截取的位置坐标
		    left = location['x']
		    top = location['y']
		    right = left + size['width']
		    bottom = top + size['height']
			#打开截图
		    i=Image.open("jietu.png")
			#使用Image的crop函数，从截图中再次截取我们需要的区域
		    image_obj =i.crop((left, top, right, bottom))
			
		    image_obj.save('yzm.png')
		    image=Image.open('yzm.png')
		    
		    #向量化处理图片  方便识别
		    image=image.convert("L")
		    #调整阈值
		    threshold=80
		    table=[]
		    for i in range(256):
		        if i <threshold:
		            table.append(0)
		        else:
		            table.append(1)
		    image=image.point(table,'1')
		#     image.show()  将图片显示出来
			#识别
		    yzm=tesserocr.image_to_text(image)
		    yzm=yzm.strip() #剔除空格
			#清空验证码输入框
		    browser.find_element_by_id("verifyCode").clear()
			#将验证码填入
		    browser.find_element_by_id("verifyCode").send_keys(yzm)
