<script context="module">

    import { ethers } from "ethers";

    export async function load({fetch, params}) {

        let hash2 = params.hash;
        console.log(params);

        let provider = new ethers.providers.AlchemyProvider();

        let transaction = await provider.getTransactionReceipt(hash2);

        return {
            props: {
                transaction
            }
        }

    }

</script>

<script>

    export let transaction;

</script>

<h2>Lazy address visualizer</h2>

<div>{ JSON.stringify(transaction) }</div>
