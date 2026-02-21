# msa-study

프로젝트 구성
util, exception 커스텀 라이브러리 (공통으로 사용하는 util method 및 exception 컨트롤 등을 구현 - 사용하는 application에 의존 주입하여 사용 개별 application의 build.gradle 파일에

dependencies {
	implementation project(':api')
	implementation project(':util')
)

와 같은 형태로 주입하여 사용

추가적으로 유틸 라이브러리의 경우 기존의 spring프로젝트와 build 방식이 달라 build.gradle 에 

plugins {
	// api 어플리케이션으로 org.springframework.boot 사용 안함
	// spring-boot-dependencies BOM 자동 적용됨
	// 라이브러리 모듈에서는 BOM 수동 적용 필요
	id "java"
	id "eclipse"
	id "io.spring.dependency-management" version "1.0.9.RELEASE"
}

---
// API application 의존
dependencies {
	implementation project(':api')
	implementation project(':util')
	implementation('org.springframework.boot:spring-boot-starter-actuator')
	implementation('org.springframework.boot:spring-boot-starter-webflux')
	testImplementation('org.springframework.boot:spring-boot-starter-test')
	testImplementation('io.projectreactor:reactor-test')
}


와 같이 org.springframework.boot 사용 안함

// BOM 수동 적용 -- 최적 버전 적용
dependencyManagement {
    imports { mavenBom("org.springframework.boot:spring-boot-dependencies:${springBootVersion}") }
}

//Boot 플러그인을 쓰면 bootJar 생성됨으로 실행 가능해짐
//라이브러리 모듈에서는 bootJar 비활성화


API applications (controller들을 interface로 생성하여 통합 API 어플리케이션에서 구현하여 사용)
통합 API application (개별 application들을 통합 구현하는 application)
