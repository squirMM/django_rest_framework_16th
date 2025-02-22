# CEOS 16기 백엔드 스터디 : TODO_MATE
### 6주차 미션 : 6주차 : AWS : EC2, RDS & Docker & Github Action

#### - docker란?
Docker는 애플리케이션을 신속하게 구축, 테스트 및 배포할 수 있는 소프트웨어 플랫폼이다.  
이 컨테이너에는 라이브러리, 시스템 도구, 코드, 런타임 등 소프트웨어를 실행하는 데 필요한 모든 것이 포함되어 있으며 Docker를 사용하면 환경에 구애받지 않고 애플리케이션을 신속하게 배포 및 확장할 수 있다.

<img width="250" alt="스크린샷 2022-11-28 오후 3 39 04" src="https://user-images.githubusercontent.com/62806067/204210382-90486cf4-fac2-4e49-a716-82e32a181a77.png">

#### - docker 동작 방식
docker는 쉽게 image를 컨테이너에 담아 실행을 시키면 해당 프로세스가 동작한다고 보면 된다.   
이때 docker image란 컨테이너를 실행할 수 잇는 실행파일, 설정값 들을 가지고 있는것이라고 생각하면된다.   
도커에서 기본적으로 제공하는 image가 아닌 내가 직접 설정하여 image를 만들고 싶다면 docker file을 생성하여 이를 통해 Image를 만들어 주면된다.   
다시말해, docker File은 이미지 생성 출발점으로 이미지를 구성하기 위한 명령어들을 작성하여 이미지를 구성할 수 있다.   

그러므로 우리는 docker file을 통해 docker image를 생성한 후 이를 컨테이너에 담아주면 원하는 프로세스가 실행되는 것이다.    

#### -  docker compose란?
compose란 툴이다.    
다시말해, 복수 개의 컨테이너를 실행시키는 도커 어플리케이션을 정의하기 위한 툴이다.   
compose를 사용하여 yaml파일로 어플리케이션의 서비스를 구성할 수 있다.   
db는 어떤것을 사용하고.... web은 어떤 것으로 사용하고 volumn은 어떻게 구성하고... 등과 같은 것들을 정의해준다.   
1. 앱의 환경을 정의하여 어디에서나 재사용 할 수 있는 Dockerfile을 정의한다.
2. docker-compose.yml 에서 앱을 재구성 할 수있는 서비스를 정의한다. (단 하나의 환경에서 실행할 수있게..)
3. docker-compose up 명령어를 싱항하여 전체 앱을 실행한다.

명령어: docker-compose -f docker-compose.yml up --build
> docker-compose -f{파일명} --{option}

#### - deploy
우리는 배포를 AWS ec2와 rds와 github action을 통해 했다.   
github action이 workflow를 자동화 시켜주면 미리 github action에 시크릿키로 저장한 ec2와 rds에 관한 정보들과    
docker의 .prod로 생성한 파일들로 action의 yml파일이 jobs를 실행시켜주면 배포가 완성된다!

#### - deploy success 
<img width="500" alt="스크린샷 2022-11-27 오전 12 05 56" src="https://user-images.githubusercontent.com/62806067/204095562-4825016f-2649-4e0e-a888-e3b9d5877f62.png">

> login api 접속 성공

#### - 총평
여태껏 햇던 과제는 그래도 좀 해봤던거라 금방 했는데    
여윽시... 배포할라니까 어리둥둥절 했다... 그동안 학교 과제하면서 조금씩햇던건데 무작정하다보니 제대로 어떻게 하는지 잘몰랐던것 같다.   
이것 저것 실수한거 투성이라 한번 제대로 정리해야겠다~ 라는 생각을 하게 되었다.   
그래도 나름 이제 배포 반은 해볼 줄 아는 사람이 된거....^^ 같아서^^... 다행이다.......   
시험기간에 깃헙 액션이나 그런 부분들이 어케 돌아가는지 한번 봐야할거같다... 끝!

*****
### 5주차 미션 : 5주차 : DRF3 - Simple JWT
JWT가 궁금해서 구글링하다가 django에서 제공하는 jwt 로직을 찾아내서 해당 부분을 사용해 봤습니다.

#### - base.py

    INSTALLED_APPS = [
        ~
        # third party
        'django_filters',
        'rest_framework',
        # # auth
        'rest_framework.authtoken',
        'dj_rest_auth',
        # # login
        'django.contrib.sites',

        'allauth',
        'allauth.account',
        'allauth.socialaccount',

        'dj_rest_auth.registration',
    ]
    
> thirdParty 추가


    REST_FRAMEWORK = {
        ~
        'DEFAULT_PERMISSION_CLASSES': (
            'rest_framework.permissions.IsAuthenticated',
            'rest_framework.permissions.AllowAny',
        ),
        'DEFAULT_AUTHENTICATION_CLASSES': (
            'rest_framework_jwt.authentication.JSONWebTokenAuthentication',
            'rest_framework.authentication.SessionAuthentication',
            'rest_framework.authentication.TokenAuthentication',
        ),
    }

> permission 및 auth 추가

#### - base 추가 변수
dj-rest-auth

    REST_USE_JWT # JWT 사용 여부, 요청값에 상세히 나오게끔!
    JWT_AUTH_COOKIE # 호출할 Cookie Key값
    JWT_AUTH_REFRESH_COOKIE# Refresh Token Cookie Key 값 (사용하는 경우)

django-allauth

    SITE_ID # 해당 도메인의 id
    ACCOUNT_EMAIL_REQUIRED # User email 필수 여부
    ACCOUNT_EMAIL_VERIFICATION# Email 인증 필수 여부

#### - url 정의

    urlpatterns = [
        ~
        path('auth/', include('dj_rest_auth.urls')),
        path('auth/registration/', include('dj_rest_auth.registration.urls')),
    ]    

> 위에 한줄만 추가하면 아래의 url로 실행가능

* http://localhost:8000/api/auth/password/reset/
* http://localhost:8000/api/auth/password/reset/confirm/
* http://localhost:8000/api/auth/login/
* http://localhost:8000/api/auth/logout/
* http://localhost:8000/api/auth/user/
* http://localhost:8000/api/auth/password/change/
* http://localhost:8000/api/auth/token/verify/
* http://localhost:8000/api/auth/token/refresh/




#### - jwt user custom

    # customize model
    USERNAME_FIELD = 'username' #'email'이라고 해주면 로그인 할때 email 써야함
    REQUIRED_FIELDS = []

    objects = UserManager()

> models.py -> user 

    class UserManager(BaseUserManager):
    def create_user(self, username, email, password, **extra_fields):
        if not email:
            raise ValueError('The Email must be set')
        if not username:
            raise ValueError('The Name must be set')
        # email = self.normalize_email(email)
        user = self.model(
            username=username,
            email=self.normalize_email(email),
            **extra_fields
        )
        user.set_password(password)
        user.save()
        return user

> managers.py

#### - accessToken , refreshToken 정의
accessToken = 말 그대로 접근 가능 하게 해주는 Token
refreshToken = accessToken 이 만료 되었을 때 accessToken 을 갱신시켜주는 Token

#### - Authentication, Authorization(Permission) 정의
Authentication(인증) = user 가 누구인지 확인
Authorization(인가) = 차등적인 권한(ex. 관리자, 사용자)을 부여할 수 있음

#### - Login 
<img width="1176" alt="스크린샷 2022-11-21 오전 1 59 03" src="https://user-images.githubusercontent.com/62806067/202915214-7a65a50c-3f01-44b7-a7fd-a9506b58b93c.png">

#### - 총평
하나 하나 구현했다면 jwt에 대해 더 깊게 이해했겠지만 viewSet을 쓴 와중에 다시 function으로 하기 싫어서 고민하던 와중 사용한 thirdparty에 대해 알게 되었다.
그래도 대략적인 구조정도는 알 수 있었고 custom 또한 어떻게 이루어지는지 이것 저것 찾다보니까 알 수 있었다.
또한 accessToken , refreshToken 의 정의와  Authentication, Authorization(Permission)에 대한 차이를 이해할 수 있게 되었다.
이번 과제가 제일 기대했던 과젠데 생각보다 thirdparty로 쉽게 만들어져서 오잉도잉했다 ㅎㅎ... 
끄읕!

*****
### 4주차 미션 : DRF2 - API View & Viewset & Filter
저번 주차에 Viewset을 사용하여 설계하였으므로 filter관련해서 정리하겠습니다.

filtering은 어떤 query set에 대하여 <b>원하는 옵션대로 필터</b>를 걸어, 특정 쿼리셋을 만들어내는 작업이라고 한다.

이번 주차에서 2가지의 방식을 사용함
1. filterset_fields - django-filter 
2. OrderingFilter - drf의 기본 필터

#### - filterset_fields
filterset_fields 를 지정해주면 equals로 비교하여 특정 쿼리셋의 결과 값을 반환해준다.

     filter_backends = (DjangoFilterBackend, OrderingFilter)
     filterset_fields = ['name', 'user', 'privacy']

하지만 자세한 비교는 불가능하여 보통 custom filterset도 많이 사용한다.

<img width="697" alt="스크린샷 2022-11-12 오후 5 40 13" src="https://user-images.githubusercontent.com/62806067/201466292-43882317-bbf0-4227-842e-cda255fdee79.png">

> content=project 쿼리 날려줌 , 부분일치와 같은 세부적인 부분은 고려 불가능

#### - OrderingFilter
OrderingFilter 에서 해당 필드를 정해주면 get에 ordering=date 이런식으로 명시해주면
date 오름차순으로 출력하여 준다.

     filter_backends = (DjangoFilterBackend, OrderingFilter)
     ordering_fields = ['date', 'like_count']
     ordering = ['date'] # default

<img width="696" alt="스크린샷 2022-11-12 오후 5 39 01" src="https://user-images.githubusercontent.com/62806067/201466255-27bf2eef-48d6-4858-bfac-d0bb826c2796.png">

> date 내림차순 정렬

#### - 총평

왜 custom filterset이 적용이 안되는지 아직 찾아내지 못했다.
그래서 아쉽긴하지만 fields로 구현해보았다.
이번에 확실히 api에 어떻게 날려야 원하는 값이 도출되는지 알게 된 것 같다.
pagination과 같은 기능들도 많던데 일단 안되는 이유 찾느라 여기 까진 고려하지 못했다.
OrderingFilter과 같이 SearchFilter도 있는데 api에 요청 날릴 때 좀 뭐랄까 명확하지 않게 전송되는거 같아서 
이 부분은 제외했다. 끝!

*****
### 3주차 미션: DRF1 - Serialize, API 설계

#### - serializes.py

     class TodoSerializer(serializers.ModelSerializer):
         user_name = serializers.SerializerMethodField()
         goal_name = serializers.SerializerMethodField()
         todo_like = serializers.SerializerMethodField()

         class Meta:
             model = Todo
             fields = ['user', 'goal', 'content', 'date', 'state', 'like_count',
                       'user_name', 'goal_name', 'todo_like']

         def get_user_name(self, obj):
             return obj.user.username

         def get_goal_name(self, obj):
             return obj.goal.name

         def get_todo_like(self, obj):
             return list(Like.objects.filter(todo_id=obj.id).prefetch_related('like_todo').values())

> 상세적인 내용을 직접 정의할 필요가 없는 ModelSerializer 사용,
> nested가 아닌 method 방식이 더 빠르다 하여 사용,
> 추후 serialize 데이터 변경이 있을지 몰라 fields에 명시,
> dto와 비슷한 느낌 받음,

#### - views.py
     
     class UserViewSet(viewsets.ModelViewSet):
         serializer_class = UserSerializer
         queryset = User.objects.all()

         def destroy(self, request, *args, **kwargs):
             user = self.get_object()
             user.is_active = False
             user.delete()
             user.save()
             return Response(data='delete user success')
             
> 간단한 로직이라 viewset 사용 및 soft delete사용으로 destory override 함 (상태변화)

#### - 데이터 출력

     # api.models.py
     class Todo(BaseModel):
         user = models.ForeignKey(User, related_name='todo_user', on_delete=models.DO_NOTHING)
         goal = models.ForeignKey(Goal, related_name='todo_goal', on_delete=models.DO_NOTHING)
         content = models.CharField(max_length=100)
         date = models.DateTimeField(default=timezone.now, help_text="날짜 및 시간")
         state = models.BooleanField(default=False)
         like_count=models.PositiveIntegerField(default=0)


<img width="452" alt="스크린샷 2022-10-07 오후 7 40 51" src="https://user-images.githubusercontent.com/62806067/194535327-928b0b57-3026-409d-9750-717d28b21d9c.png">

#### - 모든 데이터 조회 api

     GET 127.0.0.1:8000/api/todo/
     
Json 결과 값

<img width="394" alt="스크린샷 2022-10-07 오후 7 14 33" src="https://user-images.githubusercontent.com/62806067/194530842-397daffb-05ba-4da4-b3fb-58d56d63ee4d.png">

#### - 특정 데이터 조회 api

     Get 127.0.0.1:8000/api/todo/1
     
Json 결과 값

<img width="323" alt="스크린샷 2022-10-07 오후 7 16 54" src="https://user-images.githubusercontent.com/62806067/194531449-0aa93586-2e28-47d9-9a4e-6cb23e4b65cf.png">

#### - 데이터 추가 요청 api

     Post 127.0.0.1:8000/api/todo
     
Json 결과 값


<img width="500" alt="스크린샷 2022-10-07 오후 7 21 15" src="https://user-images.githubusercontent.com/62806067/194532048-4290b3e5-802d-4d8a-87ae-7e78d534c6b3.png">



#### - 데이터 삭제 요청 api

Json 결과 값

<img width="500" alt="스크린샷 2022-10-08 오전 1 45 33" src="https://user-images.githubusercontent.com/62806067/194605429-4fb52a3a-cb02-4efa-8aca-32eaa83520e6.png">

상태 변화 확인

<img width="308" alt="스크린샷 2022-10-08 오전 1 45 56" src="https://user-images.githubusercontent.com/62806067/194605459-2ea82f65-26d3-4e70-b961-d90679f8d6f2.png">

#### - 총평

1. 이번에도 역시 뭐가 맞는지 몰라 열심히 찾아봤다. 스프링이랑 비슷한가? 싶을 때 쯤 다른게 느껴진다.
2. 편하다는거 막 가져다 쓸라니까 힘들었다. 편하다고 만들어주긴했는데 좀 명시적으로 만들어 주면 좋았을텐데 싶었다 너무 지맘대로 이름을 정한기분
3. list랑 detail을 구분한다는게 요상했다. 매우.
4. 나름 열심히했는데 잘한건지 모르겠당.

*****
### 2주차 미션: DB 모델링 및 Django ORM
#### - ERD 설계 
<img src="https://user-images.githubusercontent.com/62806067/193407054-74253a1b-49ed-47fa-ba48-b622a057e3d2.png" width="800" height="400"/>

> 기초적인 기능 중심으로 설계
1. user : Django에서 기본적으로 제공하는 모델이 아닌 확장해서 사용 할 수 있는 Abstract 모델을 사용함으로 써 필요한 부분만 가져다 씀
2. goal : 목표 테이블로 todo의 카테고리 기능을 함, 공통 목표가 아닌 사용자 별로 원하는 목표가 다르므로 user을 fk로 가진 테이블을 생성
3. todo : todo 테이블로 할일이 등록되는 테이블, user와 goal을 fk로 가지고 있으며 content에는 내용, date에는 해당 날짜가 들어갈 것
4. like : 좋아요 기능을 구현하기 위한 테이블, todo와 user을 fk로 가지고 잇음
* basemodel : 생성시간, 수정시간, 삭제시간, 삭제여부 필드가 있으며 모든 테이블이 해당 모델을 사용함

#### - models.py 구현 및 migratiob
models.py 일부 발췌

     # api.models.py
     class Todo(BaseModel):
        user = models.ForeignKey(User, related_name='todo', on_delete=models.DO_NOTHING)
        goal = models.ForeignKey(Goal, related_name='todo', on_delete=models.DO_NOTHING)
        content = models.CharField(max_length=100)
        date = models.DateTimeField(default=timezone.now, help_text="날짜 및 시간")
        state = models.BooleanField(default=False)
        like_count=models.PositiveIntegerField(default=0)


> delete를 따로 설정해줘서 on_delete=models.DO_NOTHING 사용, datetime 특정 warning으로 timezone.now 사용

마이그레이션 코드

    python manage.py makemigration
    python manage.py migrate


#### - ORM 사용

<img width="600" alt="orm1" src="https://user-images.githubusercontent.com/62806067/193408520-a8555eb8-b421-4c7b-a591-7966d62dc29e.png">

> User, Goal 두개의 fk를 가지는 Todo에 3개의 객체를 집어넣음  

<img width="600" alt="orm2" src="https://user-images.githubusercontent.com/62806067/193408562-f85956ee-de0d-4a7b-9ca3-a211b6db0810.png">

> Todo를 query와 함께 queryset으로 보여줌

<img width="600" alt="orm3" src="https://user-images.githubusercontent.com/62806067/193408568-d89fd2e8-f0a0-4e75-9a17-54742c4fab6b.png">

> 다른 Goal 생성 후 Todo에 filter을 사용해서 조회함, Goal별로 조회 


#### - 총평

1. abstractuser 가 정확히 뭔지 몰라서 한참 찾았다. 애초에 만들기 전에 해주면 좋대서 했는데 아직 뭐가 그렇게 다른지 체감이..
2. enum이 text로 들어가서 제대로 조회가 안된다고 한다. 그러니까 새로 만들어 줘야하는데 여기까진 시간이 없어서 아쉬웠다.
3. 추가적인 기능을 구현할만한게 뭐가 있는지 고민했는데 워낙 단순한 서비스라 더 할게 없었다. - 굳이 해주자면 알람정도?
4. 이번엔 env로 보안 처리도 잘해주고 db도 바꿔보고 삽질과 함께 이것 저것 해봐서 재밋었다.. 나름... :)
