<script>
    import Chart from 'chart.js/auto';

    import {onMount} from 'svelte';

    let cv;
    let labels = ['Sicherheit', 'Wartbarkeit', 'Benutzbarkeit', 'Installierbarkeit', 'Verfügbarkeit']
    let targets = [50, 50, 50, 50, 50]
    let actuals = [20, 20, 20, 20, 20]
    let chart
    onMount(async (promise) => {
        console.log("ONMOUNT")
        const ctx = cv.getContext('2d');

        chart = new Chart(ctx, {
            type: 'radar',
            data: {
                labels: labels,
                datasets: [
                    {
                        label: 'Top 5 Quality Ziel-Level',
                        data: targets,
                        borderWidth: 3,
                        fill: false,
                        backgroundColor: 'rgb(75, 192, 192)',
                        borderColor: 'rgb(75, 192, 192)'
                    },
                    {
                        label: 'Actual Situation',
                        data: actuals,
                        borderWidth: 3,
                        fill: false,
                        backgroundColor: 'rgb(255, 99, 132)',
                        borderColor: 'rgb(255, 99, 132)',
                    }]
            },
            options: {
                scales: {
                    r: {
                        suggestedMin: 0,
                        suggestedMax: 100
                    }
                }
            }
        });
    });

    $: if (chart) {
        chart.data.labels = labels,
            chart.data.datasets[0].data = targets,
            chart.data.datasets[1].data = actuals,
            chart.update();
    }

    const remove = index => () => {
        labels = removeItem(labels, index)
        targets = removeItem(targets, index)
        actuals = removeItem(actuals, index)
    };

    function removeItem(arr, index) {
        const copy = [...arr];
        copy.splice(index, 1)
        return copy
    }
</script>
<div class="row">
    <div class="col-4">
        <form>
            {#each labels as l, index}
                <div class="row mb-2">
                    <div class="col-5">
                        <input bind:value={labels[index]} type="text" aria-label="Goal" class="form-control" placeholder="goal" min="0" max="100">
                    </div>
                    <div class="col">
                        <input bind:value={targets[index]} type="text" aria-label="First name" class="form-control"
                               placeholder="target">
                    </div>
                    <div class="col">
                        <input bind:value={actuals[index]} type="text" aria-label="Last name" class="form-control"
                               placeholder="actual">
                    </div>
                    <div class="col">
                        <button on:click={remove(index)} class="btn btn-outline-secondary">Remove</button>
                    </div>
                </div>
            {/each}
            <button class="btn btn-outline-primary">Add</button>
        </form>
    </div>
    <div class="col">
        <canvas bind:this={cv}/>
    </div>
</div>