<template>
  <div>
 <div class="dx-field">
            <div class="dx-field-label">Người liên quan</div>
            <div class="dx-field-value">
              <DxTagBox
                items=""
                :search-enabled="true"
                :height="90"
                placeholder=""
              />
            </div>
          </div>
  </div>
</template>

<script>

export default {
  name: "dashboard",

};
</script>

<style></style>
