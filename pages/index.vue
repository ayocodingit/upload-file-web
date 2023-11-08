<script setup>
const runtimeConfig = useRuntimeConfig();
const state = reactive({
  url: "",
});
const onClick = async (e) => {
    e.preventDefault();
   navigator.clipboard.writeText(state.url);
   alert('Your link has been copied to clipboard ')
}

const onChange = async (e) => {
  const files = e.target.files;
  const formData = new FormData();
  formData.append("file", files[0]);
  try {
    const { data } = await useFetch(
      runtimeConfig.public.apiBase + "/v1/upload",
      {
        method: "post",
        body: formData,
      }
    );
    state.url = data.value.data.url;
  } catch (error) {
    console.log(error);
  }
};
</script>

<template>
  <div>
    <h1 class="text-red-700">File Upload</h1>
    <form>
      <label for="file">File: </label>
      <input type="file" name="file" @change="onChange" />
      <br />
      <div>
        result: 
        <div v-if="state.url">
        url: {{ state.url }}
        <div>
            <button @click="onClick">Copy to clipboard</button>
        </div>
        </div>
        <div>

        </div>
      </div>
    </form>
  </div>
</template>
