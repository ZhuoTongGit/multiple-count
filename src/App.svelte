<main>
    <h1 class="title">Multiple Counter</h1>
    
    <container>
        <div class="counterList">
            {#each counterList as item, index}

                <div class="counter">
                    <input type="text" on:input={(e) => changeTitle(index, e)} value={item.title} class="counterTitle"/>
                    <span class="counterValue">{item.value}</span>
                    
                    <div class="actions">
                        <button class="add" on:click={ ()=> increase(index) }>+</button>
                        <button class="reduce" on:click={ ()=> decrease(index) }>-</button>
                        <button class="reset" on:click={ ()=> reset(index) }>0</button>
                    </div>
                    <!-- svelte-ignore a11y-click-events-have-key-events -->
                    <span class="counterRemove" on:click={ () => removeCounter(index) }>X</span>
                </div>
            {/each}

        </div>
        <!-- Add Counter Button-->
        <button class="addCounter" on:click={addCounter}>
            Add Counter
        </button>
       
        <div class="titleList">
            Title List:
            <span>{title}</span>
        </div>
        
        <div class="sum">
            Sum of Count:
            <span>{sum}</span>
        </div>
    </container>
</main>

<script lang="ts">
    let sum: number = 0
    let title: string = ""
    interface counterType {
        title: string,
        value: number
    }
    let counterList :Array<counterType> = []
    let defaultCounter = { title: 'New', value: 0}
    // initialize
    counterList.push(defaultCounter)

    const Change = () => {
        $: title = counterList.reduce((t, c) => `${t} ${c.title}`, '')
        $: sum = counterList.reduce((t, c) => t + c.value, 0)
    }
    const addCounter = () => {
        counterList = [...counterList, defaultCounter]
        Change()
    }
    const removeCounter = (idx: number) => {
        counterList = counterList.filter((value, index) => index !== idx)
        Change()
    }
    const increase = (idx: number) => {

        counterList = counterList.map((counter, index) => {
            if(index === idx)
                return {
                    title: counter.title,
                    value: counter.value + 1
                }
 
            return counter
        })
        Change()
    }
    const decrease = (idx: number) => {
        counterList = counterList.map((counter, index) => {
            if(index === idx && counter.value > 0)
                return {
                    title: counter.title,
                    value: counter.value - 1
                }
 
            return counter
        })
        Change()
    }
    const reset = (idx: number) => {
        counterList = counterList.map((counter, index) => {
            if(index === idx)
                return {
                    title: counter.title,
                    value: 0
                }
 
            return counter
        })
        Change()
    }
    const changeTitle = (idx:number, e: Event) => {
        const {target} = e
        if (target) {
            counterList = counterList.map((counter, index) => {
                if(index === idx)
                    return {
                        title: (target as HTMLInputElement).value,
                        value: 0
                    }
    
                return counter
            })
            Change()
        }
    }
</script>

<style>
    main {
        width: 100%;
        text-align: center;
        color: #ff3e00;
    }
    .title {
        font-size: 45px;
        
    }
    .addCounter {
        font-family: inherit;
        font-size: 20px;
        padding: 0.5em 2em;
        color: #ff3e00;
        background-color: rgba(255, 62, 0, 0.1);
        border-radius: 2em;
        border: 2px solid rgba(255, 62, 0, 0);
        outline: none;
        width: 400px;
        font-variant-numeric: tabular-nums;
        cursor: pointer;
    }
    .titleList {
        margin: 10px 0px;
    }
    .sum {
        font-weight: bold;
    }
    .counterList {
        text-align: center;
        margin: 25px 0px;
    }
    .counter {

        text-align: center;
        width: 400px;
        background-color: rgba(180, 223, 238, 0.25);
        padding: 0.5em 1em;
        margin:auto;
        margin-top:15px;
        border-radius: 2em;
        border: 2px solid rgba(255, 62, 0, 0);
        outline: none;
        font-variant-numeric: tabular-nums;
        display: flex;
        justify-content: space-between;
    }
    .counterTitle {
        border: 0px;
        border-radius:15px;
        padding: 5px 15px;
        color: #ff3e00;
        font-weight: bold;
    }
    .counterTitle:focus-visible {
        border: 0px;
    }
    .counterValue, .counterRemove{
        line-height: 30px;
        font-weight: bold;
    }
    .counterRemove {
        cursor: pointer;
        color: #817373;
    }
    .actions button {
        color: white;
        border: 0px;
        height:30px;
        width: 30px;
        margin:0px;
        padding: 0px;
    }
    .add {
        background-color: rgba(255, 0, 0, 0.663);
    }
    .reduce {
        background-color: rgba(0, 0, 255, 0.597);
    }
    .reset {
        background-color: #c19c1872;
    }
    .actions button {
        border-radius: 20px;
        cursor: pointer;
    }
</style>
