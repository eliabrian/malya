<template>
  <div id="app">
    <b-navbar toggleable="lg" type="dark" variant="dark">
      <b-navbar-brand to="/">Malya</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav class="ml-auto">
          <b-nav-item to="/" active>Home</b-nav-item>
          <b-nav-item to="/discover" active>Discover</b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>

    <main role="main">
      <router-view></router-view>
    </main>
  </div>
</template>

<style></style>
