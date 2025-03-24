<template>
    <nav class="navbar">
      <img src="./img/BUILD..png" alt="Logo" class="logo" />
      <div class="icons">
        <i class="fas fa-cog"></i> <!-- Icône Paramètres -->
        <i class="fas fa-user"></i> <!-- Icône Profil -->
      </div>
    </nav>
  </template>
  
  <style scoped>

  .navbar {
    background-color: black;
    color: white;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 40px 20px;
    width: 100%; 
    box-sizing: border-box;
  }
  
  .logo {
    width: 140px;
  }
  
  .icons i {
    margin-left: 20px;
    font-size: 20px;
    cursor: pointer;
  }

  </style>
  
  