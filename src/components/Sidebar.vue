<template>
  <div class="sidebar">
    <div class="sidebar-logo">
      <i class="fab fa-apple"></i>
    </div>
    <hr>
    <div class="sidebar-icons">
      <i class="fas fa-angle-double-up"></i>
      <i class="fas fa-angle-double-left"></i>
      <i class="fas fa-angle-double-down"></i>
      <i class="fas fa-angle-double-right"></i>
      <i class="fas fa-angle-up active"></i>
      <i class="fas fa-angle-left"></i>
      <i class="fas fa-angle-down"></i>
      <i class="fas fa-angle-right"></i>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style>
  .sidebar {
    display: flex;
    flex-direction: column;
    width: 75px;
    background: white;
    height: 100%;
  }
  .sidebar-logo {
    color: cadetblue;
    display: flex;
    justify-content: center;
    font-size: 32px;
    padding: 8px;
  }
  .sidebar-icons {
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #7d879c;
    font-size: 24px;
  }
  .sidebar-icons * {
    padding: 8px;
  }
  .active {
    color: #5495df;
  }
  hr {
    width: 50%;
  }
</style>