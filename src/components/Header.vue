<template>
    <div>
        <b-nav>
            <b-nav-item disabled><b>Quiz App</b></b-nav-item>
            <b-nav-item disabled>Counter: {{correctAnswers}} / {{total}}  </b-nav-item>         
        </b-nav>
    </div>
</template>

<script>
export default {
    props:[
        'correctAnswers',
        'total'
    ]
}
</script>