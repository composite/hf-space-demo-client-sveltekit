<script>
  import Chat from './Chat.svelte';

  let chatdiv;
  let chats = [];
  let msg = '';
  let form = null;

  const chatSubmit = (e) => {
    if (!form) form = e.currentTarget;
    if (!msg) {
      alert('Please input messsage.');
      requestAnimationFrame(() => form?.querySelector('input')?.focus());
      return;
    }
    form.querySelectorAll('input,button').forEach((el) => (el.disabled = true));
    chats = [...chats, msg];
    msg = '';
    requestAnimationFrame(() => chatdiv?.scrollTo(0, Number.MAX_SAFE_INTEGER));
  };

  const handleComplete = () => {
    form?.querySelectorAll('input,button').forEach((el) => (el.disabled = false));
    requestAnimationFrame(() => {
      form?.querySelector('input')?.focus();
      chatdiv?.scrollTo(0, Number.MAX_SAFE_INTEGER);
    });
  };
</script>

<main class="h-full w-full py-24">
  <h1 class="py-4 text-center font-bold md:text-2xl">
    YAME Chatbot with <a
      href="https://huggingface.co/spaces/Qwen/Qwen1.5-32B-Chat-demo"
      target="_blank"
      class="hover:underline">Qwen/Qwen1.5-32B-Chat-demo</a
    >
  </h1>
  <div
    bind:this={chatdiv}
    class="mx-auto h-full max-h-[30rem] max-w-screen-md overflow-auto rounded-xl border border-gray-400 p-4"
  >
    {#each chats as chat}
      <Chat message={chat} on:message={handleComplete} />
    {/each}
  </div>
  <form on:submit={chatSubmit} class="mx-auto flex max-w-screen-md items-center gap-4 p-2 p-4">
    <input
      type="text"
      id="chat-message"
      class="w-full flex-1 rounded border border-gray-400 p-2 disabled:bg-gray-200"
      bind:value={msg}
    />
    <button type="submit" class="rounded bg-gray-200 px-4 py-2 disabled:text-gray-400">Send</button>
  </form>
</main>
