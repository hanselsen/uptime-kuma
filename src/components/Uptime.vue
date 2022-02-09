<template>
    <span :class="className">{{ uptime }}</span>
</template>

<script>
export default {
    props: {
        monitor: Object,
        type: String,
        pill: {
            type: Boolean,
            default: false,
        },
        pct: {
            type: Boolean,
            default: true
        }
    },

    computed: {
        uptime() {

            let key = this.monitor.id + "_" + this.type;
            if(!this.pct) {
                return this.text;
            }

            if (this.$root.uptimeList[key] !== undefined) {
                return Math.round(this.$root.uptimeList[key] * 10000) / 100 + "%";
            }

            return this.$t("notAvailableShort")
        },

        color() {
            if (this.lastHeartBeat.status === 0) {
                return "danger"
            }

            if (this.lastHeartBeat.status === 1) {
                return "primary"
            }

            if (this.lastHeartBeat.status === 2) {
                return "warning"
            }

            return "secondary"
        },
        text() {
            if (this.lastHeartBeat.status === 0) {
                return this.$t("Down");
            }

            if (this.lastHeartBeat.status === 1) {
                return this.$t("Up");
            }

            if (this.lastHeartBeat.status === 2) {
                return this.$t("Pending");
            }

            return this.$t("Unknown");
        },

        lastHeartBeat() {
            if (this.monitor.id in this.$root.lastHeartbeatList && this.$root.lastHeartbeatList[this.monitor.id]) {
                return {
                    ...this.$root.lastHeartbeatList[this.monitor.id]
                };
            }

            return {
                status: -1,
            }
        },

        className() {
            if (this.pill) {
                return `badge rounded-pill bg-${this.color}`;
            }

            return "";
        },
    },
}
</script>

<style>
.badge {
    min-width: 62px;
}
</style>
