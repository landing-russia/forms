<script>
  import gsap from "gsap";
  import { onMount } from "svelte";
  import { createForm } from "svelte-forms-lib";
  import * as yup from "yup";

  const { form, errors, handleSubmit, handleChange } = createForm({
    initialValues: {
      username: "",
      password: "",
    },
    validationSchema: yup.object().shape({
      username: yup.string().required().trim(),
      password: yup.string().required().min(3).max(12).trim(),
    }),
    onSubmit: ({ username, password }) => {
      console.log("hello form", { username, password });
    },
  });

  onMount(() => {
    const tl = gsap.timeline();

    tl.from(".box", {
      y: 20,
      autoAlpha: 0,
      duration: 0.8,
      ease: "elastic.out(1, 0.3)",
      delay: 0.25,
    });
  });
</script>

<div class="container-fluid">
  <h1 class="box invisible text-4xl font-bold text-gray-800 dark:text-gray-100">
    Welcome to SvelteKit
  </h1>
  <div class="mt-16 max-w-2xl mx-auto">
    <form on:submit|preventDefault={handleSubmit}>
      <label for="username" class="block text-sm text-gray-600">Username</label>
      <input
        type="text"
        class="block w-full rounded-lg"
        name="username"
        id="username"
        placeholder="username"
        on:change={handleChange}
        bind:value={$form.username}
      />
      {#if $errors.username}
        <small class="block mt-1 mb-4 text-xs italic text-red-700 capitalize"
          >{$errors.username}</small
        >
      {/if}
      <label for="password" class="mt-6 block text-sm text-gray-600"
        >Password</label
      >
      <input
        type="password"
        class="block w-full rounded-lg"
        name="password"
        id="password"
        placeholder="password"
        on:change={handleChange}
        bind:value={$form.password}
      />
      {#if $errors.password}
        <small class="block mt-1 mb-4 text-xs italic text-red-700 capitalize"
          >{$errors.password}</small
        >
      {/if}

      <button
        type="submit"
        class="p-2 bg-blue-500 text-white mt-4 px-6 rounded-lg">Submit</button
      >
    </form>
  </div>
</div>
