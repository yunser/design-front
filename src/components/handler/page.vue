<template>
    <div ref="resizeBox" class="resize-box" :style="handlerStyle"
         @click.stop="click"
    >
        <!--{{ handlerType }}-->
        <!--{{ data.width }}，-->
        <!--{{ x }}，-->
        <!--{{ y }}，-->
        <!--{{ width }}，-->
        <!--{{ height }}-->
        <div class="center"
             @mousedown.top="handlerMouseDown($event, 'center')">
        </div>
        <!--<div class="handler handler-rotate"-->
        <!--@mousedown.top="handlerMouseDown($event, 'rotate')">-->
        <!--</div>-->
        <div class="handler handler-left"
             @mousedown.top="handlerMouseDown($event, 'left')">
        </div>
        <div class="handler handler-top"
             @mousedown.top="handlerMouseDown($event, 'top')">
        </div>
        <div class="handler handler-right"
             @mousedown.top="handlerMouseDown($event, 'right')">
        </div>
        <div class="handler handler-bottom"
             @mousedown.top="handlerMouseDown($event, 'bottom')">
        </div>
        <div class="handler handler-right-bottom"
             @mousedown.top="handlerMouseDown($event, 'right_bottom')">
        </div>
        <div class="handler handler-right-top"
             @mousedown.top="handlerMouseDown($event, 'right_top')">
        </div>
        <div class="handler handler-left-bottom"
             @mousedown.top="handlerMouseDown($event, 'left_bottom')">
        </div>
        <div class="handler handler-left-top"
             @mousedown.top="handlerMouseDown($event, 'left_top')">
        </div>
    </div>
    <!--<div class="handler-box">-->
        <!---->
    <!--</div>-->
</template>

<script>
    export default {
        data() {
            return {
                handlerType: null,
                x: 0,
                y: 0,
                width: 10,
                height: 10
            }
        },
        props: {
            data: {
                type: Object,
                default() {
                    return {
                        x: 0,
                        y: 0,
                        width: 100,
                        height: 100
                    }
                }
            },
            parentYY: {
                type: Number,
                default: 0
            },
            parentXX: {
                type: Number,
                default: 0
            }
        },
        computed: {
            handlerStyle() {
                return {
//                    top: this.y + 'px',
                    top: 0 + 'px',
                    left: 0 + 'px',
                    width: this.width + 'px',
                    height: this.height + 'px'
                }
            }
        },
        mounted() {
            document.addEventListener('mousemove', this._onMouseMove = e => {
                this.handlerMouseMove(e)
            })
            document.addEventListener('mouseup', this._onMouseUp = e => {
                this.handlerMouseUp(e)
            })
        },
        destroyed() {
            document.removeEventListener('mousemove', this._onMouseMove)
            document.removeEventListener('mouseup', this._onMouseUp)
        },
        methods: {
            click() {
            },
            handlerMouseDown(e, type) {
                this.handlerType = type
                this.isDown = true
                this.downX = e.pageX
                this.downY = e.pageY
                this.originX = this.x
                this.originY = this.y
                this.originWidth = this.width
                this.originHeight = this.height
                this.parentX = this.$refs.resizeBox.getBoundingClientRect().left
                this.parentY = this.$refs.resizeBox.getBoundingClientRect().top
            },
            handlerMouseMove(e) {
                if (this.isDown) {
                    console.log('move')
                    switch (this.handlerType) {
                        case 'top':
                            this.y = e.pageY - this.parentYY
                            this.height = this.originHeight - (e.pageY - this.downY)
                            break
                        case 'left':
                            this.x = e.pageX - this.parentXX
                            this.width = this.originWidth - (e.pageX - this.downX)
                            break
                        case 'right':
                            this.width = this.originWidth + (e.pageX - this.downX)
                            break
                        case 'bottom':
                            this.height = this.originHeight + (e.pageY - this.downY)
                            break
                        case 'left_bottom':
                            this.x = e.pageX - this.parentXX
                            this.width = this.originWidth - (e.pageX - this.downX)
                            this.height = this.originHeight + (e.pageY - this.downY)
                            break
                        case 'right_top':
                            this.width = this.originWidth + (e.pageX - this.downX)
                            this.y = e.pageY - this.parentYY
                            this.height = this.originHeight - (e.pageY - this.downY)
                            break
                        case 'right_bottom':
                            this.width = this.originWidth + (e.pageX - this.downX)
                            this.height = this.originHeight + (e.pageY - this.downY)
                            break
                        case 'left_top':
                            this.x = e.pageX - this.parentXX
                            this.width = this.originWidth - (e.pageX - this.downX)
                            this.y = e.pageY - this.parentYY
                            this.height = this.originHeight - (e.pageY - this.downY)
                            break
                        case 'center':
                            this.x = this.originX + (e.pageX - this.downX)
                            this.y = this.originY + (e.pageY - this.downY)
                            break
                    }
                    this.$emit('change', {
                        x: this.x,
                        y: this.y,
                        width: this.width,
                        height: this.height,
                        rotate: this.rotate
                    })
                }
            },
            handlerMouseUp(e) {
                this.isDown = false
            },
            setData(data) {
                console.log('设置data')
                this.x = data.x
                this.y = data.y
                this.width = data.width
                this.height = data.height
                this.rotate = data.rotate
            }
        },
        watch: {
            data: {
                deep: true,
                handler() {
                    this.x = this.data.x
                    this.y = this.data.y
                    this.width = this.data.width
                    this.height = this.data.height
                    this.rotate = this.data.rotate
                }
            }
        }
    }
</script>

<style lang="scss" scoped>
</style>
