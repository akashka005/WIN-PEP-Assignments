# ASSIGNMENT TASK-1 #

DEMO:

![alt text](image.png)
![alt text](image-1.png)

1. core/urls.py

from django.urls import path
from .views import student_list

urlpatterns = [
    path('students/', student_list),
]

2. core/models.py

from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()
    course = models.CharField(max_length=100)

    def __str__(self):
        return self.name


3. core/views.py

from rest_framework.decorators import api_view
from rest_framework.response import Response
from .models import Student

@api_view(['GET'])
def student_list(request):
    students = Student.objects.all()

    data = [
        {
            "id": student.id,
            "name": student.name,
            "age": student.age,
            "course": student.course
        }
        for student in students
    ]

    return Response(data)


# ASSIGNMENT TASK-2 #

core/views.py

from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status
from .models import Student
from .serializers import StudentSerializer

@api_view(['POST'])
def add_student(request):
    serializer = StudentSerializer(data=request.data)
    
    if serializer.is_valid():
        serializer.save()
        return Response(serializer.data, status=status.HTTP_201_CREATED)
    
    return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)


core/serializers.py

from rest_framework import serializers
from .models import Student

class StudentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Student
        fields = '__all__'


core/models.py

from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()
    course = models.CharField(max_length=100)

    def __str__(self):
        return self.name

DEMO:

![alt text](image-2.png)