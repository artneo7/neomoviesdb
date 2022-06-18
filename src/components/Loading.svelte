<div class="loading">
  <div class="loading__bar"></div>
</div>

<style>
.loading {
  display: flex;
  height: 10px;
  overflow: hidden;
}
.loading__bar {
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  background-color: none;
  width: 100%;
  background-image: linear-gradient(45deg, var(--gray-2) 25%, transparent 25%, transparent 50%, var(--gray-2) 50%, var(--gray-2) 75%, transparent 75%, transparent);
  background-size: 16px 16px;
  animation: progress-bar-stripes 1s linear infinite;
}
@keyframes progress-bar-stripes {
  from {
    background-position: 16px 0;
  }
  to {
    background-position: 0 0;
  }
}
</style>