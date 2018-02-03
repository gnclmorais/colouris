<template>
  <div></div>
</template>

<script>
export default {}
</script>

<style>
.color-row {
  width: 360px;
  display: flex;
  align-content: center;
  position: relative;
}

.color-row__label {
  width: 50px;
  flex: 0 0 50px;
  line-height: 2.2em;
  text-align: right;
}

.color-row__input {
  flex-grow: 2;
  margin-left: 10px;
}
.color-row__input,
.color-row__input:disabled {
  color: whitesmoke;
}

.color-row__button {
  width: 40px;
  margin-right: 10px;
  flex: 0 0 40px;
  padding: 0 1rem;
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
  position: relative;
  left: -40px;

  /* Overrides */
  background-color: transparent;
  border: none;
  transition: background-color 100ms ease-out;
}
.color-row__button:hover {
  /* Overrides */
  background-color: whitesmoke;
  color: black;
}

.color-row__copy-msg {
  position: absolute;
  right: -10px;
  color: whitesmoke;
  font-size: 1.1rem;
  font-family: 'Roboto', 'Helvetica Neue', 'Helvetica', 'Arial', sans-serif;
  text-transform: uppercase;
  letter-spacing: .1rem;
  font-weight: 700;
  height: 3.8rem;
  line-height: 3.8rem;

  /* Hide with: */
  visibility: hidden;
  opacity: 0;
}

.color-row__copy-msg--show {
  visibility: visible !important;
  animation: fade 1s linear;
}

@keyframes fade {
  0%, 100% { opacity: 0 }
       10% { opacity: 1 }
}
</style>
