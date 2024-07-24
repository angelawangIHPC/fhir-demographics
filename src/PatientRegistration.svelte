<script lang="ts">
    import { onMount } from "svelte";
    import { navigate } from "svelte-routing";
    import { fhir } from "./fhir";

    let loading: boolean = false;
    export let id;
    console.log(id);
    let form;
    let axiosConfig = {
  headers: {
      'Content-Type': 'application/json',
      "Access-Control-Allow-Origin": "*",
  }
};
    async function handleSubmit(e: any){
      loading = true;
      if (id){
      
      const r = await fhir.put(`/Patient/${id}`, {... e.detail, id}, axiosConfig)
      .then(r => console.log(r.data))
      .catch(err => console.log('Patient',err));
    
      } else {
        loading = true;
      const r = await fhir.post("/Patient", e.detail, axiosConfig)
      .then(r => console.log(r.data))
      .catch(err => console.log('Patient',err));
    }
    loading = false;
    navigate("/", { replace: true })
    }
    
    onMount(async () => {
      if (id){
        const r = await fhir.get(`/Patient/${id}`);
        const resource = r.data;
        form.import(resource);
      }
    });
    fhir.interceptors.response.use(
  response => response,
  error => {
    if (!error.response) {
      // We have a network error
      console.error('Network error:', error);
    }
    return Promise.reject(error);
  }
);
  </script>
  
  <h1 class="text-2xl font-semibold font-sans">Patient Registration</h1>

  <mb-fhir-form bind:this={form} 
  class="flex flex-col gap-3" on:mb-submit={handleSubmit}>
    <mb-context path="resourceType" bind="Patient"></mb-context>
    <mb-input label="Name" path="name[0].given[0]"></mb-input>
    <mb-date label="Date of birth" path="birthDate"></mb-date>
    <mb-buttons datatype="code" label="Gender" path="gender">
      <mb-option value="male" label="Male"></mb-option>
      <mb-option value="female" label="Female"></mb-option>
      <mb-option value="other" label="Other"></mb-option>
    </mb-buttons>
    <mb-submit>
      <sl-button {loading} type="info"> Submit </sl-button>
    </mb-submit>
  </mb-fhir-form>