<script>
    import { ethers } from "ethers";
    import BlocksComponent from "$lib/BlocksComponent.svelte";
    import TransactionsComponent from "$lib/TransactionsComponent.svelte";

    let alchemyProvider = new ethers.providers.AlchemyProvider();

    let blocks = [];
    let transactions = []

    async function updateBlocks() {
        let currentBlock = await alchemyProvider.getBlockWithTransactions();

        if (!(blocks.length > 0 && blocks[0].number == currentBlock.number)) {
            let newBlock = {
                number: currentBlock.number,
                hash: currentBlock.hash,
                timestamp: timeConverter(currentBlock.timestamp),
            };

            if (blocks.unshift(newBlock) > 5) {
                blocks.pop();
            }

            let currentTransactions =  currentBlock.transactions.slice(-10);

            if (currentTransactions.length !== 0) {

                let gasPriceArray = currentBlock.transactions.map(x => ethers.utils.formatEther(x.gasPrice));
                //console.log(gasPriceArray);

                currentTransactions = currentTransactions.map((x) => {
                    return {
                        hash: x.hash,
                        transactionIndex: x.transactionIndex,
                        from: x.from,
                        to: x.to,
                        isContract: x.data.length > 2 ? 'Yes': "No",

                    }
                }).reverse();

                transactions = currentTransactions;

            }

            blocks = blocks;
        }

        setTimeout(() => {
            updateBlocks();
        }, 4000);
    }

    updateBlocks();

    function timeConverter(UNIX_timestamp) {
        var a = new Date(UNIX_timestamp * 1000);
        var year = a.getFullYear();
        var month = a.getMonth();
        var date = a.getDate();
        var hour = a.getHours();
        var min = a.getMinutes();
        var sec = a.getSeconds();
        var time = year + "-" + month + "-" + date + 
            " " +
            hour +
            ":" +
            min +
            ":" +
            sec;
        return time;
    }
</script>

<body>
    <div class="header">
        <h1>Blockchain Explorer</h1>
        <p>{ethers.version}</p>
    </div>

    <div class="row">
        <div class="column">
            <h2>Latest blocks</h2>
            <BlocksComponent blocks= {blocks} />
        </div>

        <div class="column">
            <h2>Latest transactions</h2>
            <TransactionsComponent transactions= {transactions} />
            
        </div>
    </div>

    <p>
        Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation
    </p>
</body>

<style>
    @import url("https://fonts.googleapis.com/css?family=Roboto&display=swap");

    body {
        margin: 0;
        font-family: "Roboto", Arial, Helvetica, sans-serif;
    }

    /* Style the header */
    .header {
        background-color: #f1f1f1;
        padding: 20px;
        text-align: center;
    }
    /* Create three equal columns that floats next to each other */

    .row {
        display: flex;
        margin-top: 20px;
        margin-left: 100px;
        margin-right: 100px;
    }

    .column {
        flex: 50%;
    }
</style>
