<script lang="ts">
    import { onMount } from 'svelte';
    import Task from './components/Task.svelte'

    let isMenuOpened:boolean = $state(true)

    let tasks = $state<any>([]) 
    let newTask = $state<any>([{name: "", description: ""}])

    // Loading the task list from the cache, if there is one
    onMount(() => {
        const storage = localStorage.getItem('tasks')
        if(storage){
            tasks = JSON.parse(storage)
        }
    })
    

    function createTask(e: Event){
        e.preventDefault()
        if(newTask.name !== ""){
            const task = {
                name: newTask.name,
                description: newTask.description,
                id: Date.now(),
                date: (new Date().toDateString())
            }
            tasks = [...tasks, task]
            isMenuOpened = false
            newTask = [{name: "", description: ""}]
            localStorage.setItem('tasks', JSON.stringify([...tasks]))
        }
    }
    
    function deleteTask(id: number){
        const newTasks = tasks.filter((task: any) => {
            if(task.id !== id){
                return task
            }
        })
        tasks = newTasks
        localStorage.setItem('tasks', JSON.stringify(newTasks))
    }

</script>

<main class="w-full p-3 space-y-20" >
    
    <!-- Button to open menu -->
    <button onclick={() => isMenuOpened = !isMenuOpened} class="w-full rounded-md p-1.5 text-lg text-white font-semibold bg-indigo-500 cursor-pointer" >Add a new text</button>    
    
    {#if isMenuOpened}
        <section class="w-full h-full absolute top-0 left-0" >
            <!-- Black background for closing the menu -->
            <button aria-label='close menu' onclick={() => isMenuOpened = !isMenuOpened} class="w-full h-full absolute bg-black/30" ></button>

            <!-- Menu -->
            <div class="h-max absolute m-auto left-5 right-5 top-0 bottom-0 p-5 rounded bg-white" >
                <div class="flex flex-col gap-y-5" >
                    <div class="space-y-1" >
                        <p class="text-[17px] select-none" >Name</p>
                        <input bind:value={newTask.name} type="text" placeholder="text" class="w-full p-1 rounded outline-zinc-300 outline-2 focus:outline-indigo-500 focus:outline-2 " >
                    </div>
                    <div class="space-y-1" >
                        <p class="text-[17px] select-none" >Description</p>
                        <input bind:value={newTask.description} type="text" placeholder="text" class="w-full p-1 rounded outline-zinc-300 outline-2 focus:outline-indigo-500 focus:outline-2 " >
                    </div>
                    <button onclick={createTask} class="w-full rounded-md p-1.5 text-lg text-white font-semibold bg-indigo-500 cursor-pointer" >Create a new task</button>
                </div>  
            </div>
        </section>
    {/if}

    <!-- List of tasks -->
    <section>
        <ul class="w-full space-y-3" >
            {#each tasks as task}
                <Task {task} {deleteTask} />
            {:else}
                <div class="text-center text-lg" >You have no tasks...</div>
            {/each}
        </ul>
    </section>

</main>

