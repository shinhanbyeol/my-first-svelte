<script>
  const buttonOnClick = () => {
    window.location.href = "/todo";
  };
</script>

<section class="welcome-main">
  <h1>환영합니다 스벨트 투두리스트 앱 입니다.</h1>
  <button on:click={buttonOnClick}>시작하기</button>
</section>

<style>
  .welcome-main {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }
  .welcome-main h1 {
    font-size: 2rem;
    font-weight: 700;
    margin-right: 1rem;
  }
  .welcome-main button {
    padding: 0.5rem 1rem;
    border: 1px solid #333;
    border-radius: 5px;
    background-color: #fff;
    font-size: 1rem;
    font-weight: 700;
    cursor: pointer;
  }
  .welcome-main button:hover {
    background-color: #333;
    color: #fff;
  }
  .welcome-main button:active {
    background-color: #fff;
    color: #333;
  }
</style>
