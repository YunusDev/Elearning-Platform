<template>
    <template>
        <div>
            <div :data-vimeo-id="lesson.video_id" data-vimeo-width="900" v-if="lesson" id="handstick"></div>
        </div>
    </template>

    <script>
        import Axios from 'axios'
        import Swal from 'sweetalert'
        import Player from '@vimeo/player'
        export default {
            props: ['default_lesson', 'next_lesson_url'],
            data() {
                return {
                    lesson: JSON.parse(this.default_lesson)
                }
            },
            methods: {
                displayVideoEndedAlert() {
                    if(this.next_lesson_url) {
                        Swal('Yaaay ! You completed this lesson !')
                            .then(() => {
                                window.location = this.next_lesson_url
                            })
                    } else {
                        Swal('Yaaay ! You completed this series !')
                    }

                },
                completeLesson() {
                    Axios.post(`/lesson/${this.lesson.id}/complete`, {})
                        .then(resp => {
                            this.displayVideoEndedAlert()
                        })
                }
            },
            mounted() {
                const player = new Player('handstick')
                console.log('mounted')


                player.on('ended', () => {
                    this.completeLesson()
                })
            }
        }
    </script>

</template>

<script>
    export default {
        name: "Player3"
    }
</script>

<style scoped>

</style>