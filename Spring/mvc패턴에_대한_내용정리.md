### MVC 패턴

- Spring MVC는 DispatcherServlet 의 등장으로 web.xml 의 역할이 축소되었다. 이전에는 서블릿을 URL로 활용하기 위해서는 반드시 web.xml 에 등록해야 했지만, 이제는 DispatcherServlet이 해당 어플리케이션으로 들어오는 요청을 모두 핸들링 해주기 때문입니다.
- Servlet 의 모든 요청은 DispatcherServlet 으로 전달됩니다.

![3g](https://user-images.githubusercontent.com/41246605/213696664-916e5df6-cfd4-4fab-9ace-f4d4aa183c26.png)

- DispatcherServlet : 프런트 컨트롤러를 담당, 모든 HTTP 요청을 받아들여 그 밖의 오브젝트 사이의 흐름을 제어, 기본적으로 스프링 MVC의 DispatcherServlet 클래스를 그대로 적용
- HandlerMapping : 클라이언트의 요청을 바탕으로 어느 컨트롤러를 실행할지 결정
- Model : 컨트롤러에서 뷰로 넘겨줄 오브젝트를 저장하기 위한 오브젝트, HttpServletRequset 와 HttpSession 처럼 String 형 키와 오브젝트를 연결해서 오브젝트를 유지
- ViewResolver : view 이름을 바탕으로 view 오브젝트를 결정
- View : 뷰에 화면 표시 처리를 의뢰
- 비즈니스로직 : 비즈니스 로직을 실행, 애플리케이션 개발자가 비즈니스 처리 사양에 맞게 작성
- 컨트롤러 (Controller) : 클라이언트 요청에 맞는 프레젠테이션 층의 애플리케이션 처리를 실행해야 함. 애플리케이션 개발자가 애플리케이션 처리 사양에 맞게 작성
- 뷰 / JSP 등 : 클라이언트에 대해 화면 표시 처리, 자바에서는 JSP 등으로 작성하는 일이 많으며, 애플리케이션 개발자가 화면의 사양에 맞게 작성

<br />

![4g](https://user-images.githubusercontent.com/41246605/213696678-e106c228-8f8f-4048-aa75-8303f1f360e6.png)

### 동작 순서

- DispatcherServlet 은 브라우저로부터 요청을 받아들입니다.
- DispatcherServlet 은 요청된 URL 을 HandlerMapping 오브젝트에 넘기고, 호출 대상의 컨트롤러 오브젝트를 얻어 URL 에 해당하는 메서드를 실행합니다.
- 컨트롤러 오브젝트는 비즈니스 로직으로 처리를 실행하고, 그 결과를 바탕으로 뷰에 전달할 오브젝트를 Model 오브젝트에 저장합니다. 끝으로 컨트롤러 오브젝트는 처리 결과에 맞는 View 이름을 반환합니다.
- DispatcherServlet 은 컨트롤러에서 반환된 View 이름을 ViewResolver 에 전달해서 View 오브젝트를  얻습니다.
- DispatcherServlet 은 View 오브젝트에 화면 표시를 의뢰합니다.
- view 오브젝트는 해당하는 뷰를 호출해서 화면 표시를 의뢰합니다.
- 뷰는 Model 오브젝트에서 화면 표시에 필요한 오브젝트를 가져와 화면 표시 처리를 실행합니다.


