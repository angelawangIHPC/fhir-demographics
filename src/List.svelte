<script lang="ts">
    import { onMount } from "svelte";
    import { Link } from "svelte-routing";
    import { fhir } from './fhir';
    import ListItem from './ListItem.svelte';

let data = [];
onMount(async () => {
    const r = await fhir.get(`/Patient`);
    console.log(r);
    data = r.data?.entry;
});
</script>

<h1 class="text-2xl font-semibold font-sans">Patient</h1>
<Link to="patient">
    <sl-button type="primary">New Patient</sl-button>
</Link>
<div>
{#each data as patient}
<p>
    <ListItem title={patient.resource.name[0].given}/>
    <Link to={`patient/${patient.resource.id}`} class="text-blue-600 font-bold">
        {patient.resource.id},{patient.resource.name[0].given}, {patient.resource.birthDate}, {patient.resource.gender}</Link>

</p>
    <!-- svelte-ignore a11y-missing-attribute -->
     {/each}
</div>
 