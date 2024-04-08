<script>
  import { createEventDispatcher } from 'svelte';
  import { client } from '@gradio/client';

  const dispatch = createEventDispatcher();
  const markdown = import('svelte-exmarkdown');

  /**
   * @type string
   */
  export let message = '';

  const test = async () => {
    if (typeof window === 'undefined') return;
    const app = await client('Qwen/Qwen1.5-32B-Chat-demo', {
      //hf_token: 'hf_YOUR_HUGGING_FACE_API_READ_KEY_HERE',
    });
    try {
      /**
       * @type any
       */
      const result = await app.predict('/model_chat', [
        message,
        [],
        'You are a helpful assistant.'
      ]);
      console.log(result);
      return result;
    } finally {
      dispatch('message');
    }
  };
  let result = test();
</script>

<div>
  <span class="mx-2 my-4 max-w-[75%] rounded bg-orange-200 p-2">{message}</span>
</div>
<div class="flex justify-end">
  {#await result}
    <span class="mx-2 my-4 rounded bg-blue-200 p-2">...</span>
  {:then answer}
    <div class="prose mx-2 my-4 max-w-[75%] rounded bg-blue-200 p-2 [&_p]:py-2">
      {#await markdown then { default: Markdown }}
        <Markdown md={answer.data[1][0][1]} />
      {/await}
    </div>
  {:catch}
    <span class="mx-2 my-4 rounded bg-red-200 p-2">(Error!)</span>
  {/await}
</div>
