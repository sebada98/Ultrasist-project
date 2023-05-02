<template>
  <div class="bodi">
    <div class="nav p-2">
      <nav class="navbar">
        <div class="container-fluid">
          <a class="navbar-brand"
            ><img
              src="../assets/chambeaya.png"
              alt=""
              class="logo"
            />ChambeaYa!</a
          >
          <form class="d-flex" role="search">
            <input
              class="form-control me-3 px-5"
              type="search"
              placeholder="Buscar"
              aria-label="Search"
            />
            <button class="btn btn-outline-warning bg-danger" type="submit">
              Search
            </button>
          </form>
        </div>
      </nav>
    </div>
  </div>
</template>

<script>

export default {
  components: {
    
  },
};
</script>

<style scoped>
.nav {
  background-color: #ffa31a;
}
.logo {
  width: 60px;
}
.p {
  outline: red 2px solid;
}
</style>
