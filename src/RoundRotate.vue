<template>
    <div class='roundrotate'>
        <ul class='out-circle'>
            <li class='inner-ball' v-for='(item, index) in balls' :key='index'>
                <img class='inner-ball-img' :src='item'>
            </li>
        </ul>
    </div>

</template>
<script>
export default {
    name: 'RoundRotate',
    props: {
        balls: {
            type: Array,
            required: true
        },
        radius: {
            type: Number,
            default: 0.9
        },
        duration: {
            type: Number,
            default: 2
        },
        count: {
            type: Number
        }
    },
    data() {
        return {
            r: 0,   //圆盘半径
            w: 0,   //物体宽度
            centerPoint: {  //中心点坐标
                x: 0,
                y: 0
            }
        }
    },
    mounted: function() {
        this.initCircleParams()
        this.calculateCenterPosition()
        this.initBallPosition()
    },
    methods: {
        //初始化圆盘参数
        initCircleParams: function(){
            //圆盘半径参数
            if(this.radius<0 || this.radius>1) {
                this.radius = 0.9
            }
            let outCircleElement = document.querySelector('.out-circle')
            outCircleElement.style.width = this.radius*100 + '%'
            outCircleElement.style.height = this.radius*100 + '%'

            let innerBallsElement = document.querySelectorAll('.inner-ball')
            innerBallsElement.forEach((item) => {
                item.style.width = (1-this.radius)*100 + '%'
                item.style.height = (1-this.radius)*100 + '%'
                //动画时间
                item.style.animationDuration = this.duration + 's'
                //旋转圈数
                if(this.count) {
                    item.style.animationIterationCount = this.count
                }
                
            })

            let innerBallImgsElement = document.querySelectorAll('.inner-ball-img')
            innerBallImgsElement.forEach((item) => {
                //动画时间
                item.style.animationDuration = this.duration + 's'
                //旋转圈数
                if(this.count) {
                    item.style.animationIterationCount = this.count
                }
            })
        },
        //计算中心点
        calculateCenterPosition: function() {
            this.r = document.querySelector('.out-circle').offsetWidth/2
            this.w = document.querySelector('.inner-ball').offsetWidth
            this.centerPoint = {
                x: this.r,
                y: this.r
            }
        },
        //计算圆盘上物体摆放位置
        calculateBallPosition: function() {
            let num = this.balls.length
            let equalAngle = 360/num
            let ballPosition = []
            for(let i=0; i<num; i++) {
                let position = {
                    x: 0,
                    y: 0
                }
                position.x = (this.centerPoint.x + Math.sin(i*equalAngle * (Math.PI / 180)) * this.r).toFixed(2)
                position.y = (this.centerPoint.y + Math.cos(i*equalAngle * (Math.PI / 180)) * this.r).toFixed(2)
                ballPosition.push(position)
            }
            return ballPosition
        },

        //初始化圆盘上物体位置
        initBallPosition: function() {
            let ballPosition = this.calculateBallPosition()
            let ballsElement = document.querySelectorAll('.inner-ball')
            let transformOrigin = {
                x: 0,
                y: 0
            }
            ballsElement.forEach((element,key) => {
                let top = ballPosition[key].x - this.w/2
                let left = ballPosition[key].y - this.w/2
                element.style.top = top  + 'px'
                element.style.left = left + 'px'
                transformOrigin.x = this.centerPoint.x - left + 'px'
                transformOrigin.y = this.centerPoint.y - top + 'px'
                element.style.transformOrigin = transformOrigin.x + " " + transformOrigin.y
            });
        }
    }
}
</script>
<style scoped>
    ul,li {
        padding: 0;
        margin: 0;
        list-style: none;
    }
    img {
        width: auto;
        height: auto;
        max-width: 100%;
        max-height: 100%;
    }
    .roundrotate {
        position: relative;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }
    .out-circle {
        position: absolute;
        border-radius: 50%;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }
    .inner-ball {
        position: absolute;
        display: inline-block;
        animation: ballRotate linear infinite forwards;
    }
    .inner-ball-img {
        animation: ballReverse linear infinite forwards;
    }
    @keyframes ballRotate {
    to {
        transform: rotate(360deg);
    }
    }
    @keyframes ballReverse {
    to {
        transform: rotate(-360deg);
    }
    }
</style>


