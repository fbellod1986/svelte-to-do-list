<script>

	import {Toast} from 'bootstrap'
  
  
	let toastElement;
	$: console.log(toastElement)

	let opc = {texto: '', color: ''}

	let instancia;
	$: if (toastElement) {
		instancia = new Toast(toastElement)	
	}
  
	const mostarToast = (texto, color) => {
	  opc = {texto, color}
	  instancia.show()
	}
  
	let todos = [];
	let todo = { id: "", texto: "", estado: false };
  
	if (localStorage.getItem("todos")) {
	  todos = JSON.parse(localStorage.getItem("todos"));
	}
  
	$: localStorage.setItem("todos", JSON.stringify(todos));
  
	const addTodos = () => {
	  if (!todo.texto.trim()) {
		console.log("texto vacio");
		todo.texto = "";
		return;
	  }
	  todo.id = Date.now();
	  todos = [...todos, todo];
	  todo = { id: "", texto: "", estado: false };
  
	  mostarToast('Todo agregado', 'bg-primary');
	};
  
	const delTodo = (id) => {
	  todos = todos.filter((item) => item.id !== id);
	  mostarToast('Todo eliminado', 'bg-danger');
	};
  
	const editTodo = (id) => {
	  todos = todos.map((item) => {
		if (item.id === id) {
		  return {
			...item,
			estado: !item.estado,
		  };
		} else {
		  return item;
		}
	  });
	  console.log(todos);
	  mostarToast('Todo editado', 'bg-warning');
	};
  
	const classIcono = (valor) =>
	  valor ? "bi bi-arrow-clockwise" : "bi bi-check2";
  
	const classEstado = (valor) =>
	  valor ? "btn btn-success" : "btn btn-warning";
  </script>
  
  <div class="container">
	<h1 class="display-5">CRUD</h1>
	<hr />
	<h1 class="my-3 display-6">TODO</h1>
	<form on:submit|preventDefault={addTodos}>
	  <input
		type="text"
		class="form-control shadow border-0"
		bind:value={todo.texto}
		placeholder="Enter para agregar tarea"
	  />
	</form>
  
	{#each todos as item}
	  <div class="shadow my-3 p-3 lead">
		<p class={item.estado ? "text-decoration-line-through" : ""}>
		  {item.texto}
		</p>
		<button
		  class="btn btn-sm {classEstado(item.estado)}"
		  on:click={editTodo(item.id)}
		>
		  <i class={classIcono(item.estado)} />
		</button>
		<button class="btn btn-sm btn-danger" on:click={delTodo(item.id)}>
		  <i class="bi bi-trash" />
		</button>
	  </div>
	{/each}
  
	<div class="toast-container position-absolute p-3 top-0 end-0">
	  <div
		bind:this={toastElement}
		class="toast align-items-center text-white {opc.color} border-0"
		role="alert"
		aria-live="assertive"
		aria-atomic="true"
	  >
		<div class="d-flex">
		  <div class="toast-body">{opc.texto}</div>
		  <button
			type="button"
			class="btn-close btn-close-white me-2 m-auto"
			data-bs-dismiss="toast"
			aria-label="Close"
		  />
		</div>
	  </div>
	</div>
  </div>
  