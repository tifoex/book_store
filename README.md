# book_store
گزارش:
معرفی جانگو
در این گزارش، از پلتفرم جانگو برای طراحی وب اپلیکیشن استفاده می¬شود. جانگو یک پلتفرم طراحی سایت بر روی پایتون است که کاملاً رایگان و متن باز است که یک جامعه بزرگ، شگفت انگیز و دلسوز از آن پشتیبانی می¬کنند. یکی از جالب ترین جنبه های جامعه جنگو، دختران جنگو است. Django Girls جلسات و کارگاه های حضوری در سراسر جهان برگزار می کند تا به مبتدیان برنامه نویسی در پایتون و توسعه برنامه های جنگو را آموزش دهد.
نصب انوایرنمنت
برای نصب از محیط لینوکس استفاده می¬کنیم.

Touch notes.txt
Sudo apt install python3-virtualenv
Virtualenv env
Source env/bin/activate
Pip install Django
Django-admin startproject Eshop
Cd Eshop
Python3 manage.py runserver 8090
ابتدا پوشه¬ای برای پروژه ساخته و داخل آن یک فایل نوت ایجاد می¬کنیم. سپس یک محیط مجازی برای پروژه ایجاد کرده که به شکل یک اینوایرونمنت عمل می¬کند. حال داخل این محیط مجازی جانگو را نصب کرده و سپس پروژه¬ی فروشگاه آنلاین را تعریف کرده و آن را روی سرور لوکال با پورت 8090 اجرا می¬کنیم؛ خروجی به شکل زیر می¬باشد:
 
دو دستور makemigrations و migrate تغییرات اعمال شده در مدل ها را به دیتابیس اعمال می¬کند.
Python3 manage.py makemigrations
Python3 manage.py migrate
با دستور زیر یک سوپرکاربر ساخته می¬شود:
Python3 manage.py createsuperuser
 
حال با استفاده از این حساب ساخته شده به ادرس ادمین سرور رفته و وارد حساب ادمین می¬شویم.
 
جانگو از فریمورک MVC استفاده می¬کند که شامل مدل، ویو و کنترلر می¬باشد.
طراحی مدل:
 با توجه به موارد خواسته شده مدل باید چهار کلاس (جدول) اصلی داشته باشد که شامل مشتریان، محصولات، گروه محصول و سبد خرید می¬باشد.  بنابراین مدل احتمالی و جداول دیتابیس آن به صورت زیر خواهد بود. 
با دستور STARTAPP به وسیله manage.py برنامه داخلی  را می¬سازیم.
Python3 manage.py startapp store
حال به سراغ فایل ستینگ در پروژه اصلی رفته و برنامه استور را به عنوان برنامه نصب شده معرفی می¬کنیم.
 
برای پیاده سازی مدل، پس از معرفی اپ استور به عنوان برنامه های نصب شده، داخل اپ مدل، مدل¬های در نظرگرفته شده را پیاده سازی می¬کنیم. برای ساخت مدل لازم است ابتدا در فایل پایتون مدل¬های جانگو فراخوانده شود.
from django.db import models

سپس مدل¬ها به شکل زیر ساخته خواهند شد:
فیلدها:
مشتری:
class Customer(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField (max_length=50)
    phone = models.CharField(max_length=10)
    email=models.EmailField()
    password = models.CharField(max_length=100)

محصولات
class Products(models.Model):
    name = models.CharField(max_length=60)
    price= models.IntegerField(default=0)
    category= models.ForeignKey(Category,on_delete=models.CASCADE,default=1 )
    description= models.CharField(max_length=250, default='', blank=True, null= True)
    image= models.ImageField(upload_to='uploads/products/')

سفارشات
class Order(models.Model):
    product = models.ForeignKey(Products,
                                on_delete=models.CASCADE)
    customer = models.ForeignKey(Customer,
                                 on_delete=models.CASCADE)
    quantity = models.IntegerField(default=1)
    price = models.IntegerField()
    address = models.CharField (max_length=50, default='', blank=True)
    phone = models.CharField (max_length=50, default='', blank=True)
    date = models.DateField (default=datetime.datetime.today)
دسته بندی ها
class Category(models.Model):
    name= models.CharField(max_length=50)

متودها:
برای ثبت مشتریان
    def register(self):
        self.save()
ثبت سفارشات 
    def placeOrder(self):
        self.save()
دریافت سفارشات
    @staticmethod
    def get_orders_by_customer(customer_id):
        return Order.objects.filter(customer=customer_id).order_by('-date')
دریافت همه اشیاء از یک دسته بندی:
    @staticmethod
    def get_all_categories():
        return Category.objects.all()
و...
بنابراین در پیاده سازی از آی دی استفاده نشد. به طور مثال کلید مشتریان، ایمیل می¬باشد.
با دستور pip freeze requirements.txt می¬تواند تمام برنامه هایی که تا کنون استفاده شده اند را با ورژن دقیقشان در فایل requirements ذخیره کرد که بعدا با انتقال و جابجایی پروژه بتوان نسخه دقیقشان را (با pip install requiremnets.txt) بازیابی کرد.
ویو:
آن چیزی از مدل که در وب نشان داده می¬شود، ویو می¬باشد. در مرحله نخست در قسمت urls از پروژه می¬توان ادرس های مختلف را تعریف کرد و هر یک را به بخشی از پروژه اختصاص داد. به همین منظور از کتابخانه include استفاده می-کنیم:
from django.urls import path  , include
urlpatterns = [
    path('admin/', admin.site.urls),
    path('' , include('store.urls'))
]
حال با دستور بالا برای ادمین به سراغ بخش ادمین جانگو میرود و برای موارد غیر از ادمین به سراغ وب اپ استور خواهد رفت.
در حمله بعد در برنامه استور فایل urls.py را ایجاد کرده و کتابخانه های مورد نیاز را اپلود کرده و بخش¬های مد نظر برای سایت را معرفی می¬کنیم.
from django.contrib import admin
from django.urls import path
from .views.home import Index , store
from .views.signup import Signup
from .views.login import Login , logout
from .views.cart import Cart
from .views.checkout import CheckOut
from .views.orders import OrderView
from .middlewares.auth import  auth_middleware
urlpatterns = [
    path('', Index.as_view(), name='homepage'),
    path('store', store , name='store'),
    path('signup', Signup.as_view(), name='signup'),
    path('login', Login.as_view(), name='login'),
    path('logout', logout , name='logout'),
    path('cart', auth_middleware(Cart.as_view()) , name='cart'),
    path('check-out', CheckOut.as_view() , name='checkout'),
    path('orders', auth_middleware(OrderView.as_view()), name='orders'),
]
الگوهای url سه بخش دارند که بخش اول ورودی را نشان می¬دهد و بخش دوم می¬گوید در صورت دریافت این ورودی به چه چیزی مراجعه کند و بخش سوم نام آن را مشخص می¬کند.
برای ساخت مشتری توسط ساین آپ در ویو از کدهای زیر استفاده می¬کنیم:
class Signup (View):
    def get(self, request):
        return render (request, 'signup.html')

    def post(self, request):
        postData = request.POST
        first_name = postData.get ('firstname')
        last_name = postData.get ('lastname')
        phone = postData.get ('phone')
        email = postData.get ('email')
        password = postData.get ('password')
        # validation
        value = {
            'first_name': first_name,
            'last_name': last_name,
            'phone': phone,
            'email': email
        }
        error_message = None

        customer = Customer (first_name=first_name,
                             last_name=last_name,
                             phone=phone,
                             email=email,
                             password=password)
        error_message = self.validateCustomer (customer)

        if not error_message:
            print (first_name, last_name, phone, email, password)
            customer.password = make_password (customer.password)
            customer.register ()
            return redirect ('homepage')
        else:
            data = {
                'error': error_message,
                'values': value
            }
            return render (request, 'signup.html', data)

که از ارجاع به صفحه ثبت نام و دریافت اطلاعات برای ساخت یک مشتری جدید و سپس ارجاع به صفحه هوم تشکیل شده است.
پس از ساخت مشتری جدید برای وارد شدن نیز کلاس لاگین و توابع مربوط به آن به صورت زیر تعریف شده اند:
class Login(View):
    return_url = None

    def get(self, request):
        Login.return_url = request.GET.get ('return_url')
        return render (request, 'login.html')

    def post(self, request):
        email = request.POST.get ('email')
        password = request.POST.get ('password')
        customer = Customer.get_customer_by_email (email)
        error_message = None
        if customer:
            flag = check_password (password, customer.password)
            if flag:
                request.session['customer'] = customer.id

                if Login.return_url:
                    return HttpResponseRedirect (Login.return_url)
                else:
                    Login.return_url = None
                    return redirect ('homepage')
            else:
                error_message = 'Invalid !!'
        else:
            error_message = 'Invalid !!'

        print (email, password)
        return render (request, 'login.html', {'error': error_message})

def logout(request):
    request.session.clear()
    return redirect('login')



برای سبد خرید نیز:

class Cart(View):
    def get(self , request):
        ids = list(request.session.get('cart').keys())
        products = Products.get_products_by_id(ids)
        print(products)
        return render(request , 'cart.html' , {'products' : products} )

برای پرداخت:

class CheckOut(View):
    def post(self, request):
        address = request.POST.get('address')
        phone = request.POST.get('phone')
        customer = request.session.get('customer')
        cart = request.session.get('cart')
        products = Products.get_products_by_id(list(cart.keys()))
        print(address, phone, customer, cart, products)

        for product in products:
            print(cart.get(str(product.id)))
            order = Order(customer=Customer(id=customer),
                          product=product,
                          price=product.price,
                          address=address,
                          phone=phone,
                          quantity=cart.get(str(product.id)))
            order.save()
        request.session['cart'] = {}

        return redirect('cart')

همانطور که دیده می¬شود در این کلاس¬ها و توابع مرتباً به صفحاتی با پسوند .html ارجاع داده شده است. این صفحات بخش فرانت اند می¬باشند که کامل کننده view می¬باشند و در پوشه ای به نام template باید نگهداری شوند.
به طور مثال فایل html فرانت اند صفحه login به صورت زیر می¬باشد:
{% extends 'base.html' %}
{% block content %}
<div class="container">
    <div class="p-3 m-3">
        <div class="col-lg-5 rounded mx-auto border pt-4">
            <div class="text-center col">
                <img
                src="https://th.bing.com/th/id/Rd51d7c0fde6d96a31538a86cbd57f3c0?rik=%2fTJxRkU2V371QQ&riu=http%3a%2f%2ftechnologyend.com%2fwp-content%2fuploads%2f2015%2f11%2fwhy-shop-online.jpg&ehk=v1KiYT0i5U1ABCRueDSdLqnazh2BdlVD89Z4qXE0Kgs%3d&risl=&pid=ImgRaw" alt=""
                class="" style="height: 150px;">
            <hr>
            </div>
            <h3 class="alert alert-light rounded-pill" style="text-align:center" >Login</h3>
            <form action="/login" method="POST">
                {% csrf_token %}
                {% if error%}
                <div class="alert alert-danger" role="alert">
                    {{error}}
                </div>
                {% endif %}
                <!-- email -->
                <div class="form-group">
                    <label for="">Email</label>
                    <input required type="email" name="email" id=""
                    value="{{values.email}}" 
                    class=" form-control-sm form-control" placehold-er="abc@gmail.com">
                </div>
                <div class="form-group">
                    <label for="">Password</label>
                    <input type="password" 
                    name="password" 
                    id=""
                     class="form-control form-control-sm" >
                </div>
            <div class="mb-3 form-check">
                    <input type="checkbox" class="form-check-input" id="exampleCheck1">
                    <label class="form-check-label d-grid gap-2" for="exampleCheck1">Check me out</label>
                </div>
                <hr>
                <button type="submit" class="btn btn-sm btn-success col-lg-12">Login</button>
            </form>
        </div>
    </div>
</div>
{% endblock %}
فایل های html که داخل template قرار دارند در کلاس¬های view توسط render فراخوانی می¬شوند.
کنترل کننده:
کنترل کننده قلب سیستم MVC است، همه چیز را هدایت می کند. برای یک چارچوب وب، این به معنای رسیدگی به درخواست‌ها و پاسخ‌ها، راه‌اندازی اتصالات پایگاه داده و بارگیری افزونه‌ها است. این کار در جنگو به طور خودکار انجام می-شود؛ بدین منظور، جنگو یک فایل Setting را می‌خواند تا بداند چه چیزی را بارگیری و تنظیم کند و سپس یک فایل پیکربندی URL را می‌خواند که به آن می‌گوید با درخواست‌های دریافتی از مرورگرها چه کند.
فرانت اند:
برای فرانت از html استفاده شده است. بدین منظور یک سایت ساده فروش کتاب ساخته شد که صفحه اول آن به صورت زیر است:
 
 صفحات html در پوشه template نگه داری شده و مشخصات لینک¬های فرانت اند را که شامل صفحه اصلی، صفحه ورود، ثبت نام، سبد خرید و لیست سفارشات است را شامل می¬شوند.

اتصال به سرور:
برای اتصال به سرور 



