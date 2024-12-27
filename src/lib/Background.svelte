<script>
    // Background circles component
</script>

<div class="background">
    <div class="circle circle-1"></div>
    <div class="circle circle-2"></div>
    <div class="circle circle-3"></div>
    <div class="circle circle-4"></div>
</div>

<style>
    .background {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1;
        overflow: hidden;
        background: linear-gradient(to bottom right, #0f172a, #1e293b);
    }

    .circle {
        position: absolute;
        border-radius: 50%;
        filter: blur(80px);
    }

    .circle-1 {
        width: 400px;
        height: 400px;
        top: -100px;
        left: -100px;
        background: rgba(99, 102, 241, 0.15);
    }

    .circle-2 {
        width: 500px;
        height: 500px;
        top: 50%;
        right: -200px;
        background: rgba(244, 63, 94, 0.15);
    }

    .circle-3 {
        width: 300px;
        height: 300px;
        bottom: -100px;
        left: 30%;
        background: rgba(168, 85, 247, 0.15);
    }

    .circle-4 {
        width: 400px;
        height: 400px;
        top: 20%;
        left: 20%;
        background: rgba(34, 211, 238, 0.15);
    }
</style>
