# Testing to Django

## 1. Where do you put test ?
- placeholder `tests.py` added to each app or create `tests/` subdirectory 
> [!NOTE]  
> - Only Use `tests.py` or `tests/` (not both)
> - Test module must start with `test_`
> - Test directory must contain `__init__.py`

## 2. SimpleTestCase and TestCase 
- `SimpleTestCase`  = No database
- `TestCase` = with database
```python
from django.test import TestCase
from django.urls import reverse
from .models import YourModel  # เรียกใช้โมเดลที่ต้องการทดสอบ
from rest_framework.test import APITestCase, APIClient
class YourViewTestCase(TestCase):
    def setUp(self):
        # สร้างข้อมูลทดสอบในฐานข้อมูล
        YourModel.objects.create(name='Test', description='Test description')
        self.client = APIClient()

    def test_your_view(self):
        # ทดสอบการเรียกใช้งาน view ด้วยการใช้งาน client
        response = self.client.get(reverse('your_view_name'))  # เปลี่ยน 'your_view_name' เป็นชื่อของ view ของคุณ
        self.assertEqual(response.status_code, 200)  # ตรวจสอบว่ารหัสสถานะเป็น 200 (สำเร็จ)
        self.assertContains(response, 'Your expected content')  # ตรวจสอบว่า response มีข้อความหรือ element ที่ต้องการ
    
    def test_your_api_view(self):
        # ทดสอบการเรียกใช้งาน API view
        url = reverse('your_api_view_name')  # เปลี่ยน 'your_api_view_name' เป็นชื่อของ API view ของคุณ
        response = self.client.get(url)
        self.assertEqual(response.status_code, status.HTTP_200_OK)  # ตรวจสอบว่ารหัสสถานะเป็น 200 (สำเร็จ)

        # ตรวจสอบข้อมูลใน response data
        data = response.data
        self.assertEqual(len(data), 1)  # ตรวจสอบว่ามีข้อมูลที่ส่งกลับมาเพียงหนึ่งรายการ
        self.assertEqual(data[0]['name'], 'Test')  # ตรวจสอบชื่อข้อมูล
        self.assertEqual(data[0]['description'], 'Test description')  # ตรวจสอบคำอธิบายข้อมูล
```
