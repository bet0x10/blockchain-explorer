<script>

export let transactions;

</script>

<table class="styled-table">
    <thead>
        <tr>
            <th>From</th>
            <th>To</th>
            <th>Hash</th>
            <th>Tx Index</th>
        </tr>
    </thead>
    <tbody>
        {#each transactions as transaction }
            <tr>
                <td>{ transaction.from.slice(0,4) }...{transaction.from.slice(-4)}</td>
                <td>{ transaction.to.slice(0,4) }...{transaction.to.slice(-4)}</td>
                <td>{transaction.hash.slice(0,5)}...{transaction.hash.slice(-5)}</td>
                <td>{transaction.transactionIndex}</td>
            </tr>
        {/each}
    </tbody>
</table>

<style>
.styled-table {
    border-collapse: collapse;
    font-size: 0.9em;
    font-family: sans-serif;
    width: 95%;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}

.styled-table thead tr {
    background-color: #009879;
    color: #ffffff;
    text-align: left;
}

.styled-table th,
.styled-table td {
    padding: 12px 15px;
}

.styled-table tbody tr {
    border-bottom: 1px solid #dddddd;
}

.styled-table tbody tr:nth-of-type(even) {
    background-color: #f3f3f3;
}

.styled-table tbody tr:last-of-type {
    border-bottom: 2px solid #009879;
}



    
</style>