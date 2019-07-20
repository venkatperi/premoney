<template>
  <div class="container-fluid menu p-0 mt-2 mb-4">
    <div class="row">
      <div class="title col-auto float-left align-middle">
<!--        <Title width="245" height="20"/>-->
        <span class="brand">Pre-Money Calculator</span>
      </div>

      <div class="col float-right text-right">
        <a class="icon" href="https://en.wikipedia.org/wiki/Roman_numerals" target="_blank">
          <font-awesome-icon :icon="['fab', 'wikipedia-w']" size="sm"/>
        </a>
        <a class="icon" href="https://github.com/venkatperi/dec2roman" target="_blank">
          <font-awesome-icon :icon="['fab', 'github']" size="sm"/>
        </a>
      </div>

    </div>
  </div>
</template>

<script>
  import GithubLogo from 'images/github.svg';
  import Title from 'images/title.svg';

  export default {
    name: 'app-menu',
    components: {
      GithubLogo,
      Title,
    },
  }

</script>


<style lang="scss" scoped>

  .icon {
    vertical-align: middle;
    color: #333;
  }

  div.title {
    padding-top: 4px !important;
  }

  .brand {
    margin-bottom: 0;
    font-family: 'Judson', serif;
    font-size: 1.4em;
    text-transform: uppercase;
    color: #333;
  }

  .nav-masthead {
    .nav-link {
      padding: .25rem 0;
      font-weight: 700;
      color: rgba(255, 255, 255, 0.5);
      background-color: transparent;
      border-bottom: .25rem solid transparent;
      &:hover, &:focus {
      }
      + .nav-link {
        margin-left: 1rem;
      }
    }
    .active {
      color: #fff;
      border-bottom-color: #fff;
    }
  }

  .masthead-brand, .nav-masthead {
    text-align: center;
  }

  @media (min-width: 30rem) {
    .masthead-brand {
      float: left;
    }
    .nav-masthead {
      float: right;
    }
  }

</style>
