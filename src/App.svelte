<script lang="ts">
import "medblocks-ui";
import "medblocks-ui/dist/shoelace";
import { Route, Router } from "svelte-routing";
import Layout from "./Layout.svelte";
import List from "./List.svelte";
import PatientRegistration from "./PatientRegistration.svelte";
import "./tailwind.css";

</script>

<Router>
  <Layout>
    <Route path="/" component={List} />
    <Route path="patient" component={PatientRegistration} />
    <Route path="patient/:id" component={PatientRegistration} />
  </Layout>
</Router>