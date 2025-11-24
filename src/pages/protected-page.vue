<template>
  <PageLayout>
    <div class="content-layout">
      <h1 id="page-title" class="content__title">Protected Page</h1>
      <div class="content__body">
        <p id="page-description">
          <span
            >This page retrieves a <strong>protected message</strong> from an
            external API.</span
          >
          <span
            ><strong
              >Only authenticated users can access this page.</strong
            ></span
          >
        </p>
        <CodeSnippet title="Protected Message" :code="message" />
      </div>
    </div>
  </PageLayout>
</template>

<script setup>
import CodeSnippet from "@/components/code-snippet.vue";
import PageLayout from "@/components/page-layout.vue";
import { getProtectedResource } from "@/services/message.service";
import { useAuth0 } from "@auth0/auth0-vue";
import { ref } from "vue";

const { getAccessTokenSilently } = useAuth0();
const message = ref("");

const getMessage = async () => {
  try {
    const accessToken = await getAccessTokenSilently();
    const { data, error } = await getProtectedResource(accessToken);

    if (data) {
      message.value = JSON.stringify(data, null, 2);
    }

    if (error) {
      message.value = JSON.stringify(error, null, 2);
    }
  } catch (error) {
    message.value = JSON.stringify({ error: error.message }, null, 2);
  }
};

getMessage();
</script>
