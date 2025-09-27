<script lang="ts">
    import Task from './components/Task.svelte'

    let openMenu:boolean = $state(true)
    let tasks:any = $state([]) 
    let newTask:any = $state([{name: "", description: ""}])

    const storage = localStorage.getItem('tasks')
    console.log(storage)
    if(storage){
        tasks = JSON.parse(storage)
    }

    function createTask(e: Event){
        e.preventDefault()
        console.log('sda', tasks)
        if(newTask.name !== ""){
            const task = {
                name: newTask.name,
                description: newTask.description,
                id: Date.now(),
                date: (new Date().toDateString())
            }
            tasks = [...tasks, task]
            openMenu = false
            newTask = [{name: "", description: ""}]
            localStorage.setItem('tasks', JSON.stringify([...tasks, task]))
            console.log(localStorage.getItem('tasks'))
        }
    }

    function deleteTask(){

    }
    
</script>

<main class="w-full p-3 space-y-20" >

<button onclick={() => openMenu = !openMenu} class="w-full rounded-md p-1.5 text-lg text-white font-semibold bg-indigo-500 cursor-pointer" >Add a new text</button>    
    {#if openMenu}
        <section class="w-full h-full absolute top-0 left-0" >
            <button aria-label='close menu' onclick={() => openMenu = !openMenu} class="w-full h-full absolute bg-black/30" ></button>
            <div class="h-max absolute m-auto left-5 right-5 top-0 bottom-0 p-5 rounded bg-white" >
                <form onsubmit={createTask} class="flex flex-col gap-y-5" >
                    <label class="space-y-1" >
                        <p class="text-[17px] select-none" >Name</p>
                        <input bind:value={newTask.name} type="text" placeholder="text" class="w-full p-1 rounded outline-zinc-300 outline-2 focus:outline-indigo-500 focus:outline-2 " >
                    </label>
                    <label class="space-y-1" >
                        <p class="text-[17px] select-none" >Description</p>
                        <input bind:value={newTask.description} type="text" placeholder="text" class="w-full p-1 rounded outline-zinc-300 outline-2 focus:outline-indigo-500 focus:outline-2 " >
                    </label>
                    <button type='submit' class="w-full rounded-md p-1.5 text-lg text-white font-semibold bg-indigo-500 cursor-pointer" >Create a new task</button>
                </form>  
            </div>
        </section>
    {/if}

    

    <section>
        <ul class="w-full space-y-3" >
            {#each tasks as task}
                <Task {task} />
            {:else}
                <div class="text-center text-lg" >You don't have tasks...</div>
            {/each}
        </ul>
    </section>

</main>

