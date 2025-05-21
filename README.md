# Real Estate - Plataforma Digital Inmobiliaria

<p align="center">
  <img src="src/assets/logo-dark.png" alt="Logo de Inmobiliaria Logo" width="100"/>
</p>

## 🏠 Descripción del Proyecto

Inmobiliaria Logo es una plataforma digital moderna y elegante diseñada para transformar la experiencia de búsqueda y gestión de propiedades. Esta aplicación web, desarrollada con las últimas tecnologías, ofrece una interfaz intuitiva y atractiva que conecta a usuarios con sus propiedades ideales.

### ✨ Características Principales

- **Búsqueda Inteligente**: Filtros avanzados para encontrar propiedades por ubicación, tipo, precio y características
- **Galería Visual**: Presentación elegante de propiedades con imágenes de alta calidad
- **Experiencia de Usuario**: Interfaz intuitiva y responsive para todos los dispositivos
- **Autenticación Segura**: Sistema de login integrado para usuarios registrados

## 🛠️ Tecnologías Implementadas

- **Frontend**: Vue 3 con Composition API + Vite
- **Estilos**: SCSS con diseño responsive
- **Control de Versiones**: Git & GitHub
- **Despliegue**: Netlify Pages

## 🚀 Demo en Vivo

<h3>

Accede a la demo del proyecto: [Inmobiliaria Logo Demo](https://real-estate-vue-tobias-moreno.netlify.app/)

</h3>

## 📱 Capturas de Pantalla

<div style="display: flex; flex-direction: column; gap: 20px; align-items: center;">
  <img src="src/assets/images/presentation-header.png" alt="Vista principal de Inmobiliaria Logo" width="800"/>
  <img src="src/assets/images/presentation-body.png" alt="Sección de propiedades" width="800"/>
  <img src="src/assets/images/presentation-footer.png" alt="Detalles de propiedad" width="800"/>
</div>

## 💻 Instalación Local

1. **Clonar el repositorio**

   ```bash
   git clone https://github.com/TobiasEmilianoMoreno/logo-frontend.git
   cd logo-frontend
   ```

2. **Instalar dependencias**

   ```bash
   npm install
   npm install primeng primeicons chart.js
   npm install chartjs-plugin-crosshair
   npm i --save-dev @types/chartjs-plugin-crosshair
   npm install ngx-echarts echarts
   npm install -D sass-embedded
   npm install vue-router
   npm install pinia axios firebase
   npm i vite-plugin-image-optimizer
   npm install ngx-echarts echarts
   ```

3. **Iniciar el servidor de desarrollo**

   ```bash
   npm run dev
   ```

4. **Acceder a la aplicación**
   ```
   http://localhost:5174
   ```

## 📋 Estructura del Proyecto

```
src/
├── assets/
│   └── images/
├── components/
│   ├── Home/
│   │   ├── HeroSectionComponent.vue
│   │   ├── BenefitsSectionComponent.vue
│   │   └── FooterComponent.vue
│   ├── Dashboard/
│   │   ├── AreaMap.vue
│   │   ├── SalesStats.vue
│   │   └── TopProperties.vue
│   └── shared/
│       └── AppSelect.vue
├── views/
│   ├── HomeView.vue
│   └── DashboardView.vue
├── router/
│   └── index.ts
├── main.ts
└── App.vue


```

---

Desarrollado por Tobias Emiliano Moreno 2025.
