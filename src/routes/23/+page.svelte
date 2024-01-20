<script>
    import { onMount } from "svelte";

    let posts = [];

    // fetch only once, because the endpoint has stopped updating
    onMount(async () => {
        fetch(
            "https://advent.sveltesociety.dev/data/2023/day-twenty-three.json",
        )
            .then((res) => res.json())
            .then((res) => (posts = res));
    });
</script>

<h1>Day 23 - ElfNet Social</h1>
<p>
    This page displays posts from a fictional social media platform for elves.
</p>

{#each posts as post}
    <div class="post">
        <h3>{post.author}</h3>
        <p>{post.content}</p>
        <p>Likes: {post.likes}</p>

        {#each post.comments as comment}
            <div class="comment">
                <h4>{comment.author}</h4>
                <p>{comment.content}</p>
                <p>Likes: {comment.likes}</p>
            </div>
        {/each}
    </div>
{/each}

<style>
    .post {
        border: 1px solid gray;
        padding: 1rem;
        margin-bottom: 1rem;
    }

    .comment {
        border-left: 1px solid gray;
        padding-left: 1rem;
    }
</style>
