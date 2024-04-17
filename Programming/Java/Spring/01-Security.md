# Security

Тут такое дело, SpringSecurity устроен на основе servlet из java...

# Принцип работы

Какими-то механизмами, Spring перехватывает Java Servlet, и мы работаем с ним. По сути для авторазиации на servlet накидываются Java Servlet Filter. А вот фильтры накидываются спрингом. По сути мы создаем цепочку фильтров. В конце формируем ответ HttpServletRequest

DelegatingFilterProxy - мостик между servlet lifecycle и spring applicationContext
