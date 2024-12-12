<h2>프로젝트 너굴톡</h2>

<h3>📖프로젝트 소개</h3>
NeogulTalk은 Java Socket을 기반으로 한 멀티 채팅 프로그램으로, 클라이언트-서버 구조를 활용함. <br>
MySQL 데이터베이스를 통해 사용자 데이터를 관리하며, 다양한 이벤트 기반 메시징 기능을 제공함. <br>
프로젝트의 주요 목표는 실시간 멀티 유저 간의 원활한 채팅 환경을 구현하는 것. <br>

<h3>🚀주요 기능</h3>
멀티 클라이언트 지원: 다중 사용자가 서버에 동시에 접속해 채팅 가능. <br>
이벤트 기반 구조: 다양한 채팅 이벤트(로그인, 로그아웃, 메시지 전송 등) 처리. <br>
MySQL 데이터베이스 연동: 사용자 인증 및 채팅 로그 저장. <br>
LLM을 이용한 도우미: 다양한 질문 가능. <br>
확장 가능 구조: 새로운 기능 추가를 쉽게 할 수 있는 모듈화된 설계. <br>

<h3>🛠️ 설치 및 실행 조건</h3>
JAVA: JDK 23<br>
MySQL: 8.0 이상<br>
의존성 라이브러리: JDBC 드라이버, JSON 파싱 라이브러리, 암호화 라이브러리<br>
LLM: LM Studio - llama-3.2-korean-bllossom-3b<br>
<br>
<h3>📃MySQL 설정</h3>
CREATE DATABASE neogultalk;<br>
USE neogultalk;<br>
<br>
CREATE TABLE `users` (<br>
  `name` varchar(45) COLLATE utf8mb4_bin NOT NULL,<br>
  `email` varchar(45) COLLATE utf8mb4_bin NOT NULL,<br>
  `id` varchar(45) COLLATE utf8mb4_bin NOT NULL,<br>
  `pw` varchar(255) COLLATE utf8mb4_bin NOT NULL,<br>
  `status` enum('ONLINE','OFFLINE') COLLATE utf8mb4_bin DEFAULT 'OFFLINE',<br>
  PRIMARY KEY (`id`),<br>
  UNIQUE KEY `name_UNIQUE` (`name`),<br>
  UNIQUE KEY `email_UNIQUE` (`email`)<br>
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin<br>
<br>
CREATE TABLE `rooms` (<br>
  `room_id` int NOT NULL AUTO_INCREMENT,<br>
  `room_name` varchar(100) COLLATE utf8mb4_bin NOT NULL,<br>
  `is_private` tinyint(1) DEFAULT '0',<br>
  `created_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,<br>
  PRIMARY KEY (`room_id`)<br>
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin<br>
<br>
CREATE TABLE `room_users` (<br>
  `room_id` int NOT NULL,<br>
  `user_id` varchar(255) COLLATE utf8mb4_bin NOT NULL,<br>
  `joined_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,<br>
  PRIMARY KEY (`room_id`,`user_id`),<br>
  KEY `user_id` (`user_id`),<br>
  CONSTRAINT `room_users_ibfk_1` FOREIGN KEY (`room_id`) REFERENCES `rooms` (`room_id`) ON DELETE CASCADE,<br>
  CONSTRAINT `room_users_ibfk_2` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`) ON DELETE CASCADE<br>
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin<br>
<br>
CREATE TABLE `messages` (<br>
  `message_id` int NOT NULL AUTO_INCREMENT,<br>
  `room_id` int NOT NULL,<br>
  `id` varchar(45) COLLATE utf8mb4_bin NOT NULL,<br>
  `message_text` text COLLATE utf8mb4_bin NOT NULL,<br>
  `sent_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,<br>
  PRIMARY KEY (`message_id`),<br>
  KEY `room_id` (`room_id`),<br>
  KEY `id` (`id`),<br>
  CONSTRAINT `messages_ibfk_1` FOREIGN KEY (`room_id`) REFERENCES `rooms` (`room_id`),<br>
  CONSTRAINT `messages_ibfk_2` FOREIGN KEY (`id`) REFERENCES `users` (`id`)<br>
) ENGINE=InnoDB AUTO_INCREMENT=22 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin<br>
<br>
CREATE TABLE `drawings` (<br>
  `drawing_id` int NOT NULL AUTO_INCREMENT,<br>
  `room_id` int NOT NULL,<br>
  `user_id` varchar(255) COLLATE utf8mb4_bin NOT NULL,<br>
  `start_x` int NOT NULL,<br>
  `start_y` int NOT NULL,<br>
  `end_x` int NOT NULL,<br>
  `end_y` int NOT NULL,<br>
  `color_rgb` int NOT NULL,<br>
  `stroke_width` int NOT NULL,<br>
  `created_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,<br>
  PRIMARY KEY (`drawing_id`),<br>
  KEY `room_id` (`room_id`),<br>
  KEY `user_id` (`user_id`),<br>
  CONSTRAINT `drawings_ibfk_1` FOREIGN KEY (`room_id`) REFERENCES `rooms` (`room_id`),<br>
  CONSTRAINT `drawings_ibfk_2` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`)<br>
) ENGINE=InnoDB AUTO_INCREMENT=61709 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin<br>

<h3>⏳제작 기간</h3>
2024/12/03 - 2024/12/10

<h3>😢아쉬운 점</h3>
파일 업로드 기능 미구현(나중에 구현 할수도?)<br>
다른 클라이언트의 정보가 업데이트 되었을 때, 서버가 다른 클라이언트에게도 정보를 알아서 보내 주었으면 좋겠다.(서버가 쓸데없는 요청을 많이 보내게 됨)<br>
LLM 서버를 사용할 때, Python Flask 서버를 켜야 하는데, 자바에서 바로 실행되게 할 수 없나..?<br>
다음에는 웹소켓으로 해보자!
