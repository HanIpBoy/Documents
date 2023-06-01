# **Protocol**

모든 프로토콜은 성공 한다는 기준으로 작성 되었음

<br />
<br />

# **User CRUD**

## **Create**

- 이메일 인증 코드 요청 `POST`

```json
//Request URL = localhost:8080/auth/email-send
{
  "userId": "terry8408@gmail.com"
}

// Response
// 성공 시
{
    "data": null,
    "status": "succeed",
    "error": null
}

// 이미 존재하는 유저에 대해서 이메일 인증을 보낼 시
{
    "data": null,
    "status": "fail",
    "error": "User is already exists"
}
```

- 이메일 인증 코드 인증 `POST`

```json
//Request URL = localhost:8080/auth/email-verify
{
  "userId": "terry8408@gmail.com",
  "emailAuthCode":"2794"
}

// Response
//성공 시 
{
    "data": null,
    "status": "succeed",
    "error": null
	}

//실패 시 
{
    "data": null,
    "status": "fail",
    "error": null
}
// 이메일 인증코드를 틀리고 난 뒤 다시 
// 이메일 인증코드를 한번이라도 틀리면 
// 인증요청을 다시 보내야 됨

// 유저 이메일 인증 요청을 한번 보냈고, 이메일 인증코드를 처음에 틀리고 두 번째 보냈을 떄는 맞았을 때의 서버 응답
// 서버의 응답은 이렇게 보냄
{
    "data": null,
    "status": "fail",
    "error": "User is not exist"
}
```

- 회원 가입 `POST`

```json
//Request URL = localhost:8080/auth/signup
{
	"userId" : "terry8408@gmail.com",
	"password":"yeah",
	"userName":"윤재",
	"birth":"2010-10-22"
}

// Response
{
    "data": null,
    "status": "succeed",
    "error": null
}
```

- 로그인`POST`

```json
//Request URL = localhost:8080/auth/signin
{
	"userId" : "terry8408@gmail.com",
   "password":"yeah"
}

// Response
//로그인 성공 시 
{
    "data": [
        {
            "userId": "terry8408@gmail.com",
            "password": null,
            "userName": "윤재",
            "birth": null,
            "lastModifiedAt": null,
            "createdAt": null,
            "token": "eyJhbGciOiJIUzUxMiJ9.eyJwYXNzd29yZCI6IiQyYSQxMCRScGpNTExJeE52V294UE82eVI5ODNPSnRuZVpqT1dmTkZjOEZhalduVEJ4NmJ5NGRXZUVPLiIsImlzcyI6Ildhc3N1cCIsImV4cCI6MTY4NDY0MzMwMiwidXNlcklkIjoidGVycnk4NDA4QGdtYWlsLmNvbSIsImlhdCI6MTY4NDU1NjkwMn0.GhHh06DpKCcLrYOTGvJS4FM3bq5kjf19Ot1Zv_yqAH4ofCa06wXlch1UmVjKAaN2R_mZwGZwQ0qQu9VDd3F12w",
            "emailAuthCode": null
        }
    ],
    "status": "succeed",
    "error": null
}

// 로그인 실패 시
{
    "data": null,
    "status": null,
    "error": "Login Failed"
}
```
<br />
<br />

## **Retrieve**

- 전체 유저 검색 `GET`

```json
//Request URL = localhost:8080/user

// Response
{
    "data": [
        {
            "userId": "terry8408@gmail.com",
            "password": null,
            "userName": "윤재",
            "birth": "1999-09-09",
            "lastModifiedAt": "2023-05-20T15:02:11.189463",
            "createdAt": "2023-05-20T14:40:45.493376",
            "token": null,
            "emailAuthCode": null
        },
        {
            "userId": "terry8408@naver.com",
            "password": null,
            "userName": "원상",
            "birth": "1999-05-12",
            "lastModifiedAt": "2023-05-20T15:02:53.820972",
            "createdAt": "2023-05-20T14:47:49.213816",
            "token": null,
            "emailAuthCode": null
        }
    ],
    "status": "succeed",
    "error": null
}
```

- 유저 한 명 검색 `GET`

```json
//Request URL = localhost:8080/user/search/{userId}

// Response
{
    "data": [
        {
            "userId": "terry8408@gmail.com",
            "password": null,
            "userName": "윤재",
            "birth": "1999-09-09",
            "lastModifiedAt": "2023-05-20T15:02:11.189463",
            "createdAt": "2023-05-20T14:40:45.493376",
            "token": null,
            "emailAuthCode": null
        }
    ],
    "status": "succeed",
    "error": null
}
```
<br />
<br />

# **Group CRUD**

## **Create**

- 그룹장의 생성 요청 `POST`

```json
//Request URL = localhost:8080/group/create
{
	"groupName" : "축구동아리",
	"description" : "2023학년도 한성대 신규 동아리",
	"leaderId": "terry8408@gmail.com",
	"groupUsers" : [ "terry8408@gmail.com", "terry8408@naver.com", "dlwpgud225@naver.com"]
}

// Response
// 그룹 생성의 응답은 그룹원이 그룹장 한명이 있다는 것으로 설정하고 응답을 보냄
{
    "data": [
        {
            "originKey": "4028cb81883887260188388ae4640000",
            "groupName": "축구동아리",
            "description": "2023학년도 한성대 신규 동아리",
            "numOfUsers": 1,
            "leaderId": "terry8408@gmail.com",
            "lastModifiedAt": "2023-05-20T18:43:26.5716101",
            "createdAt": "2023-05-20T18:43:26.5716101",
            "groupUsers": [
                "terry8408@gmail.com"
            ]
        }
    ],
    "status": "succeed",
    "error": null
}
```

- 그룹원의 그룹 초대 수락 응답`POST`

```json
//Request URL = localhost:8080/group/invitation/accept

// 수락 시 클라의 요청
{
    "originKey": "4028cb818849996b0188499ebc6f0003", // Noti의 OriginKey
    "groupOriginKey": "402b818849996b0188499ebc0c0001"
}

// Response
{
    "data": null,
    "status": "succeed",
    "error": null
}
```

그룹원의 그룹 초대 거절 응답

```json
// Noti 삭제 API에 요청을 보내면 됨
```
<br />
<br />

## **Retrieve**

- 유저가 속한 모든 그룹 검색`GET`

```json
//Request URL = localhost:8080/group

// Response
{
    "data": [
        {
            "originKey": "4028cb81883908df0188390acadc0000",
            "groupName": "축구동아리",
            "description": "2023학년도 한성대 신규 동아리",
            "numOfUsers": 3,
            "leaderId": "terry8408@gmail.com",
            "lastModifiedAt": "2023-05-20T21:06:03.710347",
            "createdAt": "2023-05-20T21:03:08.69041",
            "groupUsers": [
                "terry8408@gmail.com",
                "terry8408@naver.com",
                "dlwpgud225@naver.com"
            ]
        },
        {
            "originKey": "4028cb8188394c2f01883959a59d0000",
            "groupName": "최강축구동아리",
            "description": "2022학년도 축구대회 우승 동아리",
            "numOfUsers": 1,
            "leaderId": "terry8408@gmail.com",
            "lastModifiedAt": "2023-05-20T22:29:16.466428",
            "createdAt": "2023-05-20T22:29:16.466428",
            "groupUsers": [
                "terry8408@gmail.com"
            ]
        }
    ],
    "status": "succeed",
    "error": null
}
```

- 특정 그룹을 검색 `GET`

```json
//Request URL = localhost:8080/group/{groupOriginKey}

//Response
{
    "data": [
        {
            "originKey": "4028cb81883908df0188390acadc0000",
            "groupName": "축구동아리",
            "description": "2023학년도 한성대 신규 동아리",
            "numOfUsers": 3,
            "leaderId": "terry8408@gmail.com",
            "lastModifiedAt": "2023-05-20T21:06:03.710347",
            "createdAt": "2023-05-20T21:03:08.69041",
            "groupUsers": [
                "이윤재",
                "김정한",
                "유영재"
            ]
        }
    ],
    "status": "succeed",
    "error": null
}
```

- 그룹원들의 ID를 그룹원 이름으로 변환 `POST`

```json
//Request URL = localhost:8080/group/search/userName
{
  "groupUsers":["cross_man@naver.com","gtrew9606@naver.com"]
}
//Response
{
  "data" : [ {
    "originKey" : null,
    "groupName" : null,
    "description" : null,
    "numOfUsers" : 0,
    "leaderId" : null,
    "lastModifiedAt" : null,
    "createdAt" : null,
    "groupUsers" : [ "유영재", "김정한" ]
  } ],
  "status" : "succeed",
  "error" : null
}

```
<br />
<br />

## **Update** `PUT`

```json
//Request URL = localhost:8080/group
// originKey와 leaderId는 반드시 포함
// 나머지는 update 할 필드만 담아서 보내도 됨
{
    "originKey": "4028cb81883db25601883dec63c90004",
    "groupName": "고촌 동아리",
    "leaderId":"dhjwdgus9332@naver.com",
    "description": "최강 고촌인들의 모임"
}

// Response

// 그룹장이 변경을 요청할 경우
{
    "data": [
        {
            "originKey": "4028cb81883db25601883dec63c90004",
            "groupName": "고촌 동아리",
            "description": "최강 고촌인들의 모임",
            "numOfUsers": 3,
            "leaderId": "dhjwdgus9332@naver.com",
            "lastModifiedAt": "2023-05-21T21:27:44.8279419",
            "createdAt": "2023-05-21T19:48:02.252663",
            "groupUsers": [
                "dhjwdgus9332@naver.com",
                "asdfjkl031@naver.com",
                "terry8408@gmail.com"
            ]
        }
    ],
    "status": "succeed",
    "error": null
}

// 그룹장이 아닌 사람이 변경을 요청 할 경우
{
    "data": null,
    "status": "fail",
    "error": "Unauthorized user is trying to access the group"
}

```
<br />
<br />

## **Delete** `DELETE`

```json
//Request URL = localhost:8080/group/{groupOriginKey}

// Response
{
    "data": [
        {
            "originKey": "4028cb8188387e7c0188387eadae0000",
            "groupName": "축구동아리",
            "description": "2023학년도 한성대 신규 동아리",
            "numOfUsers": 1,
            "leaderId": "terry8408@gmail.com",
            "lastModifiedAt": "2023-05-20T18:30:06.144807",
            "createdAt": "2023-05-20T18:30:06.144807",
            "groupUsers": []
        }
    ],
    "status": "succeed",
    "error": null
}

// 그룹장이 아닌 사람이 그룹 삭제 시
{
    "data": null,
    "status": "fail",
    "error": "Unauthorized user is trying to access the group"
}
```
<br />
<br />

# **Notification**

- 쌓인 Noti 반환 `GET`

```json
//Request URL = localhost:8080/user/notification/unread

//Response
{
    "data": [
  {
    "originKey": "abcd1234", //Noti의 OriginKey
    "userId": "terry8408@gmail.com", //해당 Noti를 받을 UserId
    "title": "그룹 초대", 
    "message": "entity.getLeaderId() + " 님이 당신을 " + entity.getGroupName() + " 그룹에 초대하셨습니다.",
    "groupOriginKey": "group123", // Noti와 관련된 groupOriginKe
    "createdAt": "2023-05-18T10:30:00"
  },
  {
    "originKey": "efgh5678",
    "userId": "jane456",
    "title": "그룹 일정 생성/수정/삭제",
    "message": "group.getGroupName() + " 그룹에서 " + groupScheduleEntity.getName() + " 일정을 생성/수정/삭제하였습니다.",
    "groupOriginKey": "group456",
    "createdAt": "2023-05-19T08:15:00"
  },
  {
    "originKey": "ijkl9012",
    "userId": "alice789",
    "title": "그룹초대",
    "message": "Notification Message 3",
    "groupOriginKey": "group789",
    "createdAt": "2023-05-20T14:45:00"
  }
],
    "status": "succeed",
    "error": null
}
```

- 특정 Noti삭제 `DELETE`

```json
//Request URL = localhost:8080/user/notification/{notificationOriginKey}

//Response
{
		// 삭제한 노티에 대한 데이터를 담아서 보냄
	}
```

- 유저의 모든 Noti 삭제 `DELETE`

```json
//Request URL = localhost:8080/user/notification

//Response
{
	data: null,
  "status": "succeed",
  "error": null
}
```
<br />
<br />

# **UserSchedule CRUD**

## **Create** `POST`

```json
//Request URL = localhost:8080/schedule
{
	"name" : "밥먹기",
	"startAt" : "2023-03-17-19:11",
	"endAt" : "2023-03-17-19:11",
	"memo" : "오늘 저녁은 고기!",   
	"allDayToggle" : "true",
  "color":"#0000FF"
}

// Response
{
    "data": [
        {
            "originKey": "4028cb818837ac26018837d29f960003",
            "name": "밥먹기",
            "startAt": "2023-03-17-19:11",
            "endAt": "2023-03-17-19:11",
            "userId": "terry8408@gmail.com",
            "memo": "오늘 저녁은 고기!",
            "allDayToggle": "true",
            "lastModifiedAt": "2023-05-20T15:22:10.3354638",
            "createdAt": "2023-05-20T15:22:10.334141",
            "color": "#0000FF"
        }
    ],
    "status": "succeed",
    "error": null
}
```
<br />
<br />

## **Retrieve** `GET`  

- 유저의 전체 일정 검색

```json
//Request URL = localhost:8080/schedule

// Response
{
    "data": [
        {
            "userId": "dhjwdgus9332@naver.com",
            "groupSchedules": [
                {
                    "originKey": "4028cb81883e3b9c01883e996a76000a",
                    "groupOriginKey": "4028cb81883db25601883dec63c90004",
                    "name": "고촌 동아리 회식",
                    "startAt": "2023-05-17T14:00:00",
                    "endAt": "2023-05-17T16:00:00",
                    "memo": "고촌 동아리의 첫 회식",
                    "allDayToggle": "false",
                    "lastModifiedAt": "2023-05-21T22:57:01.69021",
                    "createdAt": "2023-05-21T22:57:01.689784",
                    "color": "##0000FF"
                },
                {
                    "originKey": "4028cb81883e3b9c01883e9bae3d000b",
                    "groupOriginKey": "4028cb81883db25601883dec63c90004",
                    "name": "고촌 동아리 두번 째 회식",
                    "startAt": "2023-05-20T20:00:00",
                    "endAt": "2023-05-20T21:00:00",
                    "memo": "고촌 동아리의 두 번 째  회식",
                    "allDayToggle": "false",
                    "lastModifiedAt": "2023-05-21T22:59:30.109191",
                    "createdAt": "2023-05-21T22:59:30.109191",
                    "color": "##0000FF"
                },
                {
                    "originKey": "4028cb81883e3b9c01883ea31a9b000c",
                    "groupOriginKey": "4028cb81883e3b9c01883e83a3ad0004",
                    "name": "오징어 시메치기 동아리 첫 회식",
                    "startAt": "2023-05-01T14:00:00",
                    "endAt": "2023-05-01T16:00:00",
                    "memo": "오징어 시메치기 동아리 첫 회식",
                    "allDayToggle": "false",
                    "lastModifiedAt": "2023-05-21T23:07:36.609436",
                    "createdAt": "2023-05-21T23:07:36.608445",
                    "color": "##0000FF"
                }
            ],
            "userSchedules": [
                {
                    "originKey": "4028cb81883db25601883df1bc930008",
                    "name": "가평여행",
                    "startAt": "2023-06-10-00:00",
                    "endAt": "2023-06-11-00:00",
                    "userId": "dhjwdgus9332@naver.com",
                    "memo": "군대동기들과 가평여행",
                    "allDayToggle": "true",
                    "lastModifiedAt": "2023-05-21T19:53:52.660046",
                    "createdAt": "2023-05-21T19:53:52.660046",
                    "color": "#ff69b4"
                },
                {
                    "originKey": "4028cb81883db25601883df80e33000c",
                    "name": "일본 도쿄 여행",
                    "startAt": "2023-06-18-08:00",
                    "endAt": "2023-07-18-12:00",
                    "userId": "dhjwdgus9332@naver.com",
                    "memo": "원상이와 오붓한 도쿄 여행",
                    "allDayToggle": "true",
                    "lastModifiedAt": "2023-05-21T20:00:46.771727",
                    "createdAt": "2023-05-21T20:00:46.771727",
                    "color": "##644829"
                }
            ]
        }
    ],
    "status": "succeed",
    "error": null
}
```

- 유저 개인 일정들만 보여주는 API

```json
//Request URL = localhost:8080/schedule/user-schedules

//Response
 {
    "data": [
        {
            "originKey": "4028cb818837ac26018837d29f960003",
            "name": "밥먹기 -- 수정됨",
            "startAt": "2023-03-17-19:11",
            "endAt": "2023-03-17-19:11",
            "userId": "terry8408@gmail.com",
            "memo": "오늘 저녁은 고기! -- 수정",
            "allDayToggle": "true",
            "lastModifiedAt": "2023-05-20T15:38:56.435819",
            "createdAt": "2023-05-20T15:22:10.334141",
            "color": "#0000CC"
        },
        {
            "originKey": "4028cb818837ac26018837d7424a0004",
            "name": "풋살 가기 -- 수정됨",
            "startAt": "2023-04-18-19:00",
            "endAt": "2023-04-18-22:00",
            "userId": "terry8408@gmail.com",
            "memo": "이번 주는 풋살 가고 싶은데 -- 수정됨",
            "allDayToggle": "false",
            "lastModifiedAt": "2023-05-20T15:42:56.980982",
            "createdAt": "2023-05-20T15:27:14.131253",
            "color": "#0000AA"
        }
    ],
    "status": "succeed",
    "error": null
}
```
<br />
<br />

## **Update** `PUT`

```json
//Request URL = localhost:8080/schedule
// originKey는 반드시 포함되어야 하고 
// 나머지 항목은 업데이트 하지 않을꺼면 안 넣어도 됨
{
    "originKey": "4028cb818837ac26018837d29f960003",
    "name": "밥먹기 -- 수정됨",
    "startAt": "2023-03-17-19:11",
    "endAt": "2023-03-17-19:11",
    "memo": "오늘 저녁은 고기! -- 수정",
    "allDayToggle": "true",
    "lastModifiedAt": "2023-05-20T15:22:10.335464",
    "createdAt": "2023-05-20T15:22:10.334141",
    "color": "#0000CC"  
}

// Response
{
    "data": [
        {
            "originKey": "4028cb818837ac26018837d29f960003",
            "name": "밥먹기 -- 수정됨",
            "startAt": "2023-03-17-19:11",
            "endAt": "2023-03-17-19:11",
            "userId": "terry8408@gmail.com",
            "memo": "오늘 저녁은 고기! -- 수정",
            "allDayToggle": "true",
            "lastModifiedAt": "2023-05-20T15:38:56.4358189",
            "createdAt": "2023-05-20T15:22:10.334141",
            "color": "#0000CC"
        }
    ],
    "status": "succeed",
    "error": null
}
```
<br />
<br />

## **Delete** `DELETE`

```json
//Request URL = localhost:8080/schedule/{originKey}
//userschedule의 originKey

// Response
// 삭제한 스케쥴의 내용을 보여줌
{
    "data": [
        {
            "originKey": "4028cb818837ac26018837d29f960003",
            "name": "밥먹기 -- 수정됨",
            "startAt": "2023-03-17-19:11",
            "endAt": "2023-03-17-19:11",
            "userId": "terry8408@gmail.com",
            "memo": "오늘 저녁은 고기! -- 수정",
            "allDayToggle": "true",
            "lastModifiedAt": "2023-05-20T15:38:56.4358189",
            "createdAt": "2023-05-20T15:22:10.334141",
            "color": "#0000CC"
        }
    ],
    "status": "succeed",
    "error": null
}
```
<br />
<br />

# **GroupSchedule CRUD**

## **Create** `POST`

```json
//Request URL = localhost:8080/group/schedule
{
  "groupOriginKey": "4028cb81883908df0188390acadc0000",
  "name": "축구동아리 회식",
  "startAt": "2023-05-17T14:00:00",
  "endAt": "2023-05-17T16:00:00",
  "memo": "축구동아리의 첫 회식",
  "allDayToggle": "false",
  "color": "##0000FF"
}

// Response
// 생성한 그룹 공통 스케쥴 한 개를 보여줌
{
    "data": [
        {
            "originKey": "4028cb81883908df01883920139b0006",
            "groupOriginKey": "4028cb81883908df0188390acadc0000",
            "name": "축구동아리 회식",
            "startAt": "2023-05-17T14:00:00",
            "endAt": "2023-05-17T16:00:00",
            "memo": "축구동아리의 첫 회식",
            "allDayToggle": "false",
            "lastModifiedAt": "2023-05-20T21:26:23.5243774",
            "createdAt": "2023-05-20T21:26:23.5233729",
            "color": "##0000FF"
        }
    ],
    "status": "succeed",
    "error": null
}

// 그룹장이 아닌 사람이 요청 할 경우
{
    "data": null,
    "status": "fail",
    "error": "Unauthorized user is trying to access the group"
}
```
<br />
<br />

## **Retrieve**

- 그룹의 공통 스케쥴 1개 검색 `GET`

```json
//Request URL = localhost:8080/
아직 안 만듦 
```

- 그룹의 공통 스케쥴들만 검색 `GET`

```json
//Request URL = localhost:8080/group/schedule/{originKey}
//그룹의 originKey 

// Response
{
    "data": [
        {
            "originKey": "4028cb81883908df01883920139b0006",
            "groupOriginKey": "4028cb81883908df0188390acadc0000",
            "name": "축구동아리 회식",
            "startAt": "2023-05-17T14:00:00",
            "endAt": "2023-05-17T16:00:00",
            "memo": "축구동아리의 첫 회식",
            "allDayToggle": "false",
            "lastModifiedAt": "2023-05-20T21:26:23.524377",
            "createdAt": "2023-05-20T21:26:23.523373",
            "color": "##0000FF"
        },
        {
            "originKey": "4028cb81883908df018839215d2f0007",
            "groupOriginKey": "4028cb81883908df0188390acadc0000",
            "name": "축구동아리 두 번째 회식",
            "startAt": "2023-05-02T14:00:00",
            "endAt": "2023-05-02T16:00:00",
            "memo": "축구동아리의 두번쨰  회식",
            "allDayToggle": "false",
            "lastModifiedAt": "2023-05-20T21:27:47.887709",
            "createdAt": "2023-05-20T21:27:47.887709",
            "color": "##0000FF"
        }
    ],
    "status": "succeed",
    "error": null
}
```

- 그룹원들의 개인 스케쥴만 검색 `GET`

```json
//Request URL = localhost:8080/group/{groupOriginKey}/user-schedules
//group의 OriginKey

{
    "data": [
        {
            "originKey": "4028cb818837ac26018837d29f960003",
            "name": "밥먹기 -- 수정됨",
            "startAt": "2023-03-17-19:11",
            "endAt": "2023-03-17-19:11",
            "userId": "terry8408@gmail.com",
            "memo": "오늘 저녁은 고기! -- 수정",
            "allDayToggle": "true",
            "lastModifiedAt": "2023-05-20T15:38:56.435819",
            "createdAt": "2023-05-20T15:22:10.334141",
            "color": "#0000CC"
        },
        {
            "originKey": "4028cb818837ac26018837d7424a0004",
            "name": "풋살 가기 -- 수정됨",
            "startAt": "2023-04-18-19:00",
            "endAt": "2023-04-18-22:00",
            "userId": "terry8408@gmail.com",
            "memo": "이번 주는 풋살 가고 싶은데 -- 수정됨",
            "allDayToggle": "false",
            "lastModifiedAt": "2023-05-20T15:42:56.980982",
            "createdAt": "2023-05-20T15:27:14.131253",
            "color": "#0000AA"
        },
        {
            "originKey": "4028cb818837e1cf0188380c9ef70001",
            "name": "배 벅벅 긁기",
            "startAt": "2023-04-01-19:11",
            "endAt": "2023-04-02-19:11",
            "userId": "dlwpgud225@naver.com",
            "memo": "어 좋다~",
            "allDayToggle": "false",
            "lastModifiedAt": "2023-05-20T16:25:31.258954",
            "createdAt": "2023-05-20T16:25:31.258954",
            "color": "#0000FF"
        }
    ],
    "status": "succeed",
    "error": null
}
```

- 그룹의 모든 스케쥴 검색 `GET` (그룹원의 모든 스케쥴 + 그룹원의 그룹 공통 스케쥴 검색)

```json
//Request URL = localhost:8080/group/user/schedule/{groupOriginKey}

{
    "data": [
        {
            "userId": "dhjwdgus9332@naver.com",
            "groupSchedules": [
                {
                    "originKey": "4028cb81883e3b9c01883e996a76000a",
                    "groupOriginKey": "4028cb81883db25601883dec63c90004",
                    "name": "고촌 동아리 회식",
                    "startAt": "2023-05-17T14:00:00",
                    "endAt": "2023-05-17T16:00:00",
                    "memo": "고촌 동아리의 첫 회식",
                    "allDayToggle": "false",
                    "lastModifiedAt": "2023-05-21T22:57:01.69021",
                    "createdAt": "2023-05-21T22:57:01.689784",
                    "color": "##0000FF"
                },
                {
                    "originKey": "4028cb81883e3b9c01883e9bae3d000b",
                    "groupOriginKey": "4028cb81883db25601883dec63c90004",
                    "name": "고촌 동아리 두번 째 회식",
                    "startAt": "2023-05-20T20:00:00",
                    "endAt": "2023-05-20T21:00:00",
                    "memo": "고촌 동아리의 두 번 째  회식",
                    "allDayToggle": "false",
                    "lastModifiedAt": "2023-05-21T22:59:30.109191",
                    "createdAt": "2023-05-21T22:59:30.109191",
                    "color": "##0000FF"
                }
            ],
            "userSchedules": [
                {
                    "originKey": "4028cb81883db25601883df1bc930008",
                    "name": "가평여행",
                    "startAt": "2023-06-10-00:00",
                    "endAt": "2023-06-11-00:00",
                    "userId": "dhjwdgus9332@naver.com",
                    "memo": "군대동기들과 가평여행",
                    "allDayToggle": "true",
                    "lastModifiedAt": "2023-05-21T19:53:52.660046",
                    "createdAt": "2023-05-21T19:53:52.660046",
                    "color": "#ff69b4"
                },
                {
                    "originKey": "4028cb81883db25601883df80e33000c",
                    "name": "일본 도쿄 여행",
                    "startAt": "2023-06-18-08:00",
                    "endAt": "2023-07-18-12:00",
                    "userId": "dhjwdgus9332@naver.com",
                    "memo": "원상이와 오붓한 도쿄 여행",
                    "allDayToggle": "true",
                    "lastModifiedAt": "2023-05-21T20:00:46.771727",
                    "createdAt": "2023-05-21T20:00:46.771727",
                    "color": "##644829"
                }
            ]
        },
        {
            "userId": "asdfjkl031@naver.com",
            "groupSchedules": [
                {
                    "originKey": "4028cb81883e3b9c01883e996a76000a",
                    "groupOriginKey": "4028cb81883db25601883dec63c90004",
                    "name": "고촌 동아리 회식",
                    "startAt": "2023-05-17T14:00:00",
                    "endAt": "2023-05-17T16:00:00",
                    "memo": "고촌 동아리의 첫 회식",
                    "allDayToggle": "false",
                    "lastModifiedAt": "2023-05-21T22:57:01.69021",
                    "createdAt": "2023-05-21T22:57:01.689784",
                    "color": "##0000FF"
                },
                {
                    "originKey": "4028cb81883e3b9c01883e9bae3d000b",
                    "groupOriginKey": "4028cb81883db25601883dec63c90004",
                    "name": "고촌 동아리 두번 째 회식",
                    "startAt": "2023-05-20T20:00:00",
                    "endAt": "2023-05-20T21:00:00",
                    "memo": "고촌 동아리의 두 번 째  회식",
                    "allDayToggle": "false",
                    "lastModifiedAt": "2023-05-21T22:59:30.109191",
                    "createdAt": "2023-05-21T22:59:30.109191",
                    "color": "##0000FF"
                }
            ],
            "userSchedules": [
                {
                    "originKey": "4028cb81883db25601883df407890009",
                    "name": "코타키나발루 여행",
                    "startAt": "2023-06-10-08:00",
                    "endAt": "2023-06-16-12:00",
                    "userId": "asdfjkl031@naver.com",
                    "memo": "레저",
                    "allDayToggle": "true",
                    "lastModifiedAt": "2023-05-21T19:56:22.922971",
                    "createdAt": "2023-05-21T19:56:22.922971",
                    "color": "##50bcdf"
                },
                {
                    "originKey": "4028cb81883db25601883df748e2000b",
                    "name": "일본 도쿄 여행",
                    "startAt": "2023-06-18-08:00",
                    "endAt": "2023-07-18-12:00",
                    "userId": "asdfjkl031@naver.com",
                    "memo": "아키하바라",
                    "allDayToggle": "true",
                    "lastModifiedAt": "2023-05-21T19:59:56.258396",
                    "createdAt": "2023-05-21T19:59:56.258396",
                    "color": "##800000"
                }
            ]
        },
        {
            "userId": "terry8408@gmail.com",
            "groupSchedules": [
                {
                    "originKey": "4028cb81883e3b9c01883e996a76000a",
                    "groupOriginKey": "4028cb81883db25601883dec63c90004",
                    "name": "고촌 동아리 회식",
                    "startAt": "2023-05-17T14:00:00",
                    "endAt": "2023-05-17T16:00:00",
                    "memo": "고촌 동아리의 첫 회식",
                    "allDayToggle": "false",
                    "lastModifiedAt": "2023-05-21T22:57:01.69021",
                    "createdAt": "2023-05-21T22:57:01.689784",
                    "color": "##0000FF"
                },
                {
                    "originKey": "4028cb81883e3b9c01883e9bae3d000b",
                    "groupOriginKey": "4028cb81883db25601883dec63c90004",
                    "name": "고촌 동아리 두번 째 회식",
                    "startAt": "2023-05-20T20:00:00",
                    "endAt": "2023-05-20T21:00:00",
                    "memo": "고촌 동아리의 두 번 째  회식",
                    "allDayToggle": "false",
                    "lastModifiedAt": "2023-05-21T22:59:30.109191",
                    "createdAt": "2023-05-21T22:59:30.109191",
                    "color": "##0000FF"
                }
            ],
            "userSchedules": [
                {
                    "originKey": "4028cb81883db25601883df95e09000d",
                    "name": "맨시티 경기",
                    "startAt": "2023-05-22-00:00",
                    "endAt": "2023-05-22-03:00",
                    "userId": "terry8408@gmail.com",
                    "memo": "맨시티 vs 첼시",
                    "allDayToggle": "false",
                    "lastModifiedAt": "2023-05-21T20:02:12.745442",
                    "createdAt": "2023-05-21T20:02:12.745442",
                    "color": "##4aa8d8"
                },
                {
                    "originKey": "4028cb81883db25601883df5ca8c000a",
                    "name": "졸업작품 제출-- 수정",
                    "startAt": "2023-05-26-18:00",
                    "endAt": "2023-05-26-21:00",
                    "userId": "terry8408@gmail.com",
                    "memo": "깃허브 레포 제출일-- 수정",
                    "allDayToggle": "false",
                    "lastModifiedAt": "2023-05-21T20:05:29.87392",
                    "createdAt": "2023-05-21T19:58:18.380642",
                    "color": "##FFFFFF"
                }
            ]
        }
    ],
    "status": "succeed",
    "error": null
}

```
<br />
<br />


## **Update** `PUT`

```json
//Request URL = localhost:8080/group/schedule
// originKey와,groupOriginKey는 반드시 포함
// 다른 필드는 Update 하고 싶은 필드만 추가해서 보내주면 됨
{
    "originKey": "4028cb81883908df01883920139b0006",
    "groupOriginKey": "4028cb81883908df0188390acadc0000",
    "name": "축구동아리 회식 -- 수정됨",
    "startAt": "2023-05-10T18:00:00",
    "endAt": "2023-05-10T20:00:00",
    "memo": "축구동아리의 첫 회식--수정됨",
    "allDayToggle": "true",
    "color": "##000000"
}
// Response 
//수정한 그룹 공통일정 내용 보여줌
{
    "data": [
        {
            "originKey": "4028cb81883908df01883920139b0006",
            "groupOriginKey": "4028cb81883908df0188390acadc0000",
            "name": "축구동아리 회식 -- 수정됨",
            "startAt": "2023-05-10T18:00:00",
            "endAt": "2023-05-10T20:00:00",
            "memo": "축구동아리의 첫 회식--수정됨",
            "allDayToggle": "true",
            "lastModifiedAt": null,
            "createdAt": null,
            "color": "##000000"
        }
    ],
    "status": "succeed",
    "error": null
}

// 그룹장이 아닌 사람이 요청 할 경우
{
    "data": null,
    "status": "fail",
    "error": "Unauthorized user is trying to access the group"
}
```
<br />
<br />


## **Delete** `DELETE`

```json
//Request URL = localhost:8080/group/schedule/{originKey}

// Response 
//삭제한 그룹 공통일정 내용 보여줌
{
    "data": [
        {
            "originKey": "4028cb81883908df01883920139b0006",
            "groupOriginKey": "4028cb81883908df0188390acadc0000",
            "name": "축구동아리 회식 -- 수정됨",
            "startAt": "2023-05-10T18:00:00",
            "endAt": "2023-05-10T20:00:00",
            "memo": "축구동아리의 첫 회식--수정됨",
            "allDayToggle": "true",
            "lastModifiedAt": "2023-05-20T21:46:46.391294",
            "createdAt": null,
            "color": "##000000"
        }
    ],
    "status": "succeed",
    "error": null
}

// 그룹장이 아닌 사람이 요청 할 경우
{
    "data": null,
    "status": "fail",
    "error": "Unauthorized user is trying to access the group"
}
```