<template>
    <div class="card" :class="[{ 'selected': selected, 'hidden': hidden }]" @click="selectCard">
        <div class="card__front">
            <span>?</span>
        </div>
        <div class="card__back">
            <span>{{ value }}</span>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        value: {
            type: Number,
            required: true
        },
        index: {
            type: Number,
            required: true
        },
        selected: {
            type: Boolean,
            required: true
        },
        hidden: {
            type: Boolean,
            required: true
        }
    },
    setup({ index }, { emit }) {
        const selectCard = () => emit('selectCard', index)

        return {
            selectCard
        }
    }
}
</script>

<style lang="scss" scoped>
.card {
    width: 100px;
    height: 150px;
    border-radius: 10px;
    position: relative;
    overflow: hidden;
    transition: all 500ms ease-in-out;

    &__front,
    &__back {
        backface-visibility: hidden;
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        transition: all 500ms ease-in-out;
        display: grid;
        place-content: center;
        color: #f8fafc;
        font-family: "Ubuntu", sans-serif;
        font-weight: 700;
        font-size: 5rem;
        transition: all 500ms ease-in-out;
        transform-style: preserve-3d;
    }

    &__front {
        cursor: pointer;
        background: #e6398f;
    }
    &__back {
        background: #41fa56;
        transform: rotateY(180deg);
    }

    &.selected {
        pointer-events: none;
        .card__front {
            transform: rotateY(180deg);
        }

        .card__back {
            transform: rotateY(0);
        }
    }

    &.hidden {
        opacity: 0;
        pointer-events: none;
    }
}
</style>