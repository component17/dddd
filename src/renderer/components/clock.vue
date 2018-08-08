<template>
    <div class="clock">
        <div class="hours">
            <div class="first">
                <div class="number">0</div>
            </div>
            <div class="second">
                <div class="number">0</div>
            </div>
        </div>
        <div class="tick">:</div>
        <div class="minutes">
            <div class="first">
                <div class="number">0</div>
            </div>
            <div class="second">
                <div class="number">0</div>
            </div>
        </div>
        <div class="tick">:</div>
        <div class="seconds">
            <div class="first">
                <div class="number">0</div>
            </div>
            <div class="second infinite">
                <div class="number">0</div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                hoursContainer: null,
                minutesContainer: null,
                secondsContainer: null,
                tickElements: null,
                last: null,
                tickState: null
            }
        },
        mounted() {
            this.hoursContainer = document.querySelector('.hours');
            this.minutesContainer = document.querySelector('.minutes');
            this.secondsContainer = document.querySelector('.seconds');
            this.tickElements = Array.from(document.querySelectorAll('.tick'));

            this.last = new Date(0);
            this.last.setUTCHours(-1);

            this.tickState = true;

            setInterval(this.updateTime, 100)
        },
        methods: {
            tick () {
                tickElements.forEach(t => t.classList.toggle('tick-hidden'))
            },

            updateTime () {
                let now = new Date;

                let lastHours = this.last.getHours().toString();
                let nowHours = now.getHours().toString();
                if (lastHours !== nowHours) {
                    this.updateContainer(this.hoursContainer, nowHours)
                }

                let lastMinutes = this.last.getMinutes().toString();
                let nowMinutes = now.getMinutes().toString();
                if (lastMinutes !== nowMinutes) {
                    this.updateContainer(this.minutesContainer, nowMinutes)
                }

                let lastSeconds = this.last.getSeconds().toString();
                let nowSeconds = now.getSeconds().toString();
                if (lastSeconds !== nowSeconds) {
                    //tick()
                    this.updateContainer(this.secondsContainer, nowSeconds)
                }

                this.last = now
            },
            updateContainer (container, newTime) {
                let time = newTime.split('');

                if (time.length === 1) {
                    time.unshift('0')
                }


                let first = container.firstElementChild
                if (first.lastElementChild.textContent !== time[0]) {
                    this.updateNumber(first, time[0])
                }

                let last = container.lastElementChild
                if (last.lastElementChild.textContent !== time[1]) {
                    this.updateNumber(last, time[1])
                }
            },
            updateNumber (element, number) {
                //element.lastElementChild.textContent = number
                let second = element.lastElementChild.cloneNode(true)
                second.textContent = number

                element.appendChild(second)
                element.classList.add('move')

                setTimeout(function () {
                    element.classList.remove('move')
                }, 990)
                setTimeout(function () {
                    element.removeChild(element.firstElementChild)
                }, 990)
            }
        }
    }
</script>

<style>
    .clock {
        height: 20vh;
        color: white;
        font-size: 22vh;
        font-family: sans-serif;
        line-height: 20.4vh;
        display: flex;
        position: relative;
        /*background: green;*/
        overflow: hidden;
    }

    .clock::before, .clock::after {
        content: '';
        width: 7ch;
        height: 3vh;
        background: linear-gradient(to top, transparent, #303030);
        position: absolute;
        z-index: 2;
    }

    .clock::after {
        bottom: 0;
        background: linear-gradient(to bottom, transparent, #303030);
    }

    .clock > div {
        display: flex;
    }

    .tick {
        line-height: 17vh;
    }

    .tick-hidden {
        opacity: 0;
    }

    .move {
        animation: move linear 1s infinite;
    }

    @keyframes move {
        from {
            transform: translateY(0vh);
        }
        to {
            transform: translateY(-20vh);
        }
    }
</style>