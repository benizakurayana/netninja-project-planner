<template>
    <form @submit.prevent="handleSubmit">
        <label>Title</label>
        <input type="text" v-model="title" required>
        <label>Details</label>
        <textarea v-model="details" required></textarea>
        <button>Update project</button>
    </form>
</template>

<script>
export default {
    data() {
        return {
            title: '',
            details: '',
            uri: 'http://localhost:3000/projects/' + this.id
        }
    },
    props: ['id'],
    mounted() {
        fetch(this.uri)
            .then(res => res.json())
            .then(data => {
                this.title = data.title
                this.details = data.details
            })

    },
    methods: {
        handleSubmit() {
            fetch(this.uri, {
                method: 'PATCH',
                headers: { 'Content-type': 'application/json' },
                body: JSON.stringify({ title: this.title, details: this.details })
            })
                .then(() => this.$router.push('/'))
                .catch(err => console.log(err.message))
        }
    }
}
</script>

<style></style>