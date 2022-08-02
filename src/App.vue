<template>
  <div id="app">
    <router-view />
  </div>
</template>

<style lang="scss">
@font-face {
  font-family: 'Satisfy';
  font-style: normal;
  font-weight: 400;
  src: local('Satisfy Regular'), local('Satisfy-Regular'), url(./font/rP2Hp2yn6lkG50LoCZOIHQ.woff2) format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
// url(font/rP2Hp2yn6lkG50LoCZOIHQ.woff2)
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
#app {
  width: 100%;
  height: 100vh;
  padding: 0px 0 0px;
  overflow: hidden;
  font-size: 14px;
  background-color: rgb(78, 55, 55);
  // background-image: repeating-linear-gradient(
  //     0deg,
  //     hsla(340, 87%, 75%, 0.2) 0px,
  //     hsla(340, 87%, 75%, 0.2) 30px,
  //     transparent 30px,
  //     transparent 60px
  //   ),
  //   repeating-linear-gradient(
  //     90deg,
  //     hsla(340, 87%, 75%, 0.2) 0px,
  //     hsla(340, 87%, 75%, 0.2) 30px,
  //     transparent 30px,
  //     transparent 60px
  //   ),
  //   linear-gradient(90deg, rgb(255, 255, 255), rgb(255, 255, 255));
  display: flex;
  justify-content: center;
  align-items: center;

  font-family: Satisfy, serif;

}

</style>
