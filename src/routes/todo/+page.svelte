<script lang="ts">
  type JOB = {
    id: number;
    content: string;
    startDate: string;
    endDate: string;
    isDone: boolean;
    alert?: boolean;
  };
  let todoList: JOB[] = [
    {
      id: 1,
      content: "집가서 빨래하기",
      startDate: "2023-09-25",
      endDate: "2023-09-25",
      isDone: false,
      alert: false,
    },
  ];
  let doneList: JOB[] = [];

  function deadLineAlert(todoList: JOB[]) {
    todoList.forEach((job) => {
      if (job.endDate !== "기한없음") {
        const today = new Date().toISOString().slice(0, 10);
        if (job.endDate < today) {
          job.alert = true;
        }
      }
    });
  }

  $: deadLineAlert(todoList);

  function addJob() {
    const content = document.getElementsByName(
      "todoWork"
    )[0] as HTMLTextAreaElement;
    const startDate = document.getElementsByName(
      "stDate"
    )[0] as HTMLInputElement;
    const endDate = document.getElementsByName("edDate")[0] as HTMLInputElement;

    if (content.value === "") {
      alert("할 일을 입력해주세요.");
      return;
    }

    const job: JOB = {
      id: todoList.length + 1,
      content: content.value,
      startDate: startDate.value || new Date().toISOString().slice(0, 10),
      endDate: endDate.value || "기한없음",
      isDone: false,
    };
    todoList = [...todoList, job];
  }

  function changeDone(jobId: number) {
    const job = todoList.find((job) => job.id === jobId);
    if (job) {
      job.isDone = !job.isDone;
      if (job.isDone) {
        doneList = [...doneList, job];
        todoList = todoList.filter((job) => job.id !== jobId);
      } else {
        todoList = [...todoList, job];
        doneList = doneList.filter((job) => job.id !== jobId);
      }
    }
  }
</script>

<section class="todo-main">
  <h1>Svelte TodoList</h1>
  <p>오늘 할 일을 적어보세요.</p>
  <section class="inputs">
    <textarea name="todoWork" />
    <div class="confirm-box">
      <p>
        <lable class="lable">시작일</lable><input name="stDate" type="date" />
      </p>
      <p>
        <lable class="lable">종료일</lable><input name="edDate" type="date" />
      </p>
      <button on:click={addJob}></button>
    </div>
  </section>
  <section class="job-list">
    <div class="todo">
      <h2>할 일 목록</h2>
      <div>
        {#each todoList as todo}
          <div class="job-item-todo">
            <div>
              <input
                type="checkbox"
                on:change={() => {
                  changeDone(todo.id);
                }}
              />
            </div>
            <div>
              {todo.alert === true ? `📣 기한을 확인하세요 ` : ""}{todo.content}
            </div>
            <div>
              {todo.startDate} ~ {todo.endDate}
            </div>
          </div>
        {/each}
        {#if todoList.length === 0}
          <p>할 일이 없습니다.</p>
        {/if}
        {#each doneList as done}
          <div class="job-item-done">
            <div>✅</div>
            <div style="text-decoration: line-through;">
              {done.content}
            </div>
            <div>
              {done.startDate} ~ {done.endDate}
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section>
</section>

<style>
  .lable {
    margin-right: 0.5rem;
  }
  .todo-main {
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100vh;
    width: 90vw;
    max-width: 800px;
  }
  .inputs {
    width: 100%;
    display: flex;
    align-items: center;
    padding: 1rem;
    border-radius: 14px;
    /* like a glass */
    background-color: rgba(255, 255, 255, 0.5);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    border: 1px solid #33333340;
  }
  .inputs textarea {
    flex: 1;
    height: 100px;
    resize: none;
    border: 1px solid #333;
    border-radius: 5px;
    padding: 0.5rem;
    font-size: 1rem;
    font-weight: 700;
  }
  .inputs .confirm-box {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-left: 1rem;
  }
  .confirm-box p {
    padding: 0;
    margin: 0;
    margin-bottom: 8px;
  }
  .inputs button {
    width: 100%;
    padding: 0.5rem 1rem;
    border: 1px solid #333;
    border-radius: 5px;
    background-color: #fff;
    font-size: 1rem;
    font-weight: 700;
    cursor: pointer;
  }
  .job-list {
    margin-top: 16px;
    display: flex;
    justify-content: center;
    width: 100%;
    flex: 1;
  }
  .job-list h2 {
    text-align: center;
  }
  .todo {
    flex: 1;
    margin-right: 1rem;
  }
  .divide {
    width: 1px;
    background-color: #333;
    height: 100%;
  }
  .done {
    flex: 1;
    margin-left: 1rem;
  }
  button:hover {
    background-color: #333;
    color: #fff;
  }
  button:active {
    background-color: #fff;
    color: #333;
  }
  .job-item-todo {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.5rem 1rem;
    border: solid 1px #33333340;
    /* beautifull box */
    box-shadow: 0 0 4px rgba(0, 0, 0, 0.6);
    /* rainbow gradient */
    background: linear-gradient(90deg, #000, #333);
    border-radius: 5px;
    margin-bottom: 0.5rem;
    color: #fff;
  }
  .job-item-done {
    color: #33333390;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.5rem 1rem;
    border: solid 1px #33333340;
    /* beautifull box */
    box-shadow: 0 0 4px rgba(0, 0, 0, 0.6);
    /* rainbow gradient */
    background: linear-gradient(
      90deg,
      #ff000040,
      #ff800040,
      #ffff0040,
      #00ff0040,
      #00ffff40,
      #0000ff40,
      #ff00ff40
    );
    border-radius: 5px;
    margin-bottom: 0.5rem;
  }
</style>
