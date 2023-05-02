<template>
  <div class="home">
    <header class="header">
      <nav class="nav">
        <a href="#" class="nav__logo">YourLogo</a>
        <ul class="nav__menu">
          <li class="nav__item"><a href="#" class="nav__link">Inicio</a></li>
          <li class="nav__item"><a href="#" class="nav__link">Servicios</a></li>
          <li class="nav__item"><a href="#" class="nav__link">Nosotros</a></li>
          <li class="nav__item"><a href="#" class="nav__link">Contacto</a></li>
        </ul>
      </nav>
    </header>
    <main class="main">
      <section class="hero">
        <h1 class="hero__title">Bienvenido a nuestra página</h1>
        <p class="hero__subtitle">Aquí encontrarás lo que necesitas</p>
        <a href="#" class="hero__cta">Comienza ahora</a>
      </section>
      <section class="services">
        <h2 class="services__title">Nuestros servicios</h2>
        <div class="services__container">
          <div class="service">
            <img src="/path/to/your/image1.jpg" alt="Service 1" class="service__image">
            <h3 class="service__name">Servicio 1</h3>
            <p class="service__description">Descripción del servicio 1</p>
          </div>
          <div class="service">
            <img src="/path/to/your/image2.jpg" alt="Service 2" class="service__image">
            <h3 class="service__name">Servicio 2</h3>
            <p class="service__description">Descripción del servicio 2</p>
          </div>
          <div class="service">
            <img src="/path/to/your/image3.jpg" alt="Service 3" class="service__image">
            <h3 class="service__name">Servicio 3</h3>
            <p class="service__description">Descripción del servicio 3</p>
          </div>
        </div>
      </section>
      <section class="about">
        <h2 class="about__title">Acerca de nosotros</h2>
        <p class="about__description">
          Somos una empresa comprometida con nuestros clientes, ofreciendo productos y servicios de alta calidad. Nuestro equipo de profesionales está siempre dispuesto a brindar la mejor atención y soluciones a sus necesidades.
        </p>
      </section>
    </main>
    <footer class="footer">
      <p class="footer__text">&copy; 2023 YourCompanyName. Todos los derechos reservados.</p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'HomeView'
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Roboto', sans-serif;
  background-color: #222;
  color: #fff;
}
.home {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.header {
  width: 100%;
  height: 80px;
  background-color: #333;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 2rem;
}

.nav {
  display: flex;
  align-items: center;
}

.nav__logo {
  font-weight: 700;
  font-size: 1.5rem;
  color: #fff;
  text-decoration: none;
}

.nav__menu {
  display: flex;
  list-style: none;
  margin-left: 2rem;
}

.nav__item {
  margin-right: 1rem;
}

.nav__link {
  font-weight: 500;
  color: #fff;
  text-decoration: none;
  transition: color 0.3s ease;
}

.nav__link:hover {
  color: #ccc;
}

.main {
  flex-grow: 1;
}

.hero {
  text-align: center;
  padding: 4rem 0;
  background-color: #444;
}

.hero__title {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.hero__subtitle {
  font-size: 1.5rem;
  margin-bottom: 2rem;
}

.hero__cta {
  background-color: #333;
  color: #fff;
  font-weight: 700;
  font-size: 1.1rem;
  text-decoration: none;
  padding: 0.8rem 2rem;
  border-radius: 2rem;
  transition: background-color 0.3s ease;
}

.hero__cta:hover {
  background-color: #555;
}

.services {
  padding: 4rem 0;
  background-color: #222;
  text-align: center;
}

.services__title {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.services__container {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}

.service {
  max-width: 300px;
  margin-bottom: 2rem;
}

.service__image {
  width: 100%;
  height: auto;
}

.service__name {
  font-size: 1.5rem;
  margin: 1rem 0;
}

.service__description {
  font-size: 1rem;
  line-height: 1.5;
}

.about {
  padding: 4rem 0;
  background-color: #444;
  text-align: center;
}

.about__title {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.about__description {
  font-size: 1rem;
  line-height: 1.5;
  max-width: 800px;
  margin: 0 auto;
}

.footer {
  background-color: #333;
  padding: 2rem 0;
  text-align: center;
}

.footer__text {
  font-size: 1rem;
}
</style>