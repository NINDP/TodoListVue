<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<style>
.btn {
    border: none;
    padding: 10px 20px 10px;
    border-radius: 4px;
    cursor: pointer;
    padding-top: 10px;
    font-size: 20px;
}
</style>