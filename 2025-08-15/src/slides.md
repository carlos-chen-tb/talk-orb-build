---
# You can also start simply with 'default'
theme: seriph
# theme: default

# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: images/b01.jpg
# background: https://cover.sli.dev

# themeConfig:
#   primary: '#e0f00'

# some information about your slides (markdown enabled)
title: Talk orb-build
info: |
  ## Slidev Starter Template
  Build Docker images faster and more cost-effectively

  Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# Talk orb-build

Build container images faster and cost-effectively

<div class="pt-12">
  <span class="text-s px-0 py-0 rounded" hover="bg-white bg-opacity-10">
    Carlos Chen / Matus Kacmar
  </span>
  <span class="text-xs rounded cursor-pointer" hover="bg-white bg-opacity-10">
    @2024-10-28
  </span>
</div>

<style>
h1 {
  background-image: linear-gradient(45deg, #4EC5D4 10%, #fff 20%) !important;
}
</style>
---
transition: fade-out
---

# What's orb?

<span v-mark.circle.orange="4">Reusable CircleCI command or job</span> to use in your [.circleci/config.yml](https://github.com/third-bridge/expert-hub/blob/edc42fbabdf65c3af3fe4cb4338e6d31fdea8720/.circleci/build.yml)

```yaml {all|4-5|13-20|all}
version: 2.1

orbs: 
  #! Install VSCode CircleCI extension to show all orb parameters or visit https://circleci.com/developer/orbs/orb/thirdbridge/build
  build: thirdbridge/build@0.1.13

workflows:
  build:
    jobs:
      - build/node_image_build:
          context: ThirdBridge # To provides FONTAWESOME_NPM_AUTH_TOKEN and GITHUB_ORG_PACKAGES_READ_TOKEN

          enable_slack_notify: true
          channel: artemis-notify

          node_version: 22.16.0 # Auto detect .nvmrc, or "nvm use ..."
          package_manager: pnpm # Auto detect packageManager and lock files, or "corepack prepare ..."
          circleci_cache_version: v1
          dockerfile: "Dockerfile.production"
          ...
```

<style>
.slidev-code {
  background: #c5eff090 !important;
  backdrop-filter: blur(10px);
  border: 1px solid #eee1;
}
</style>
---
level: 2
---

# Real-world usage

In CircleCI Web UI

<img src="/expert-hub_node-image-build_circleci_ui.png" w-full />

---

# Real-world usage

Slack notify enabled

<img src="/expert-hub_node-image-build_slack_notify.png" w-full />
---

# Why a new orb?

Back to [2025-02](https://splunk.test10.pro/en-US/app/search/search?q=search%20index%3D%22ci%22%20sourcetype%3D%22circleci_metric%22%0A%20%20%20%20%0Aearliest%3D%2202%2F01%2F2025%3A00%3A00%3A00%22%20latest%3D%2203%2F01%2F2025%3A00%3A00%3A00%22%0A%0AJOB_BUILD_STATUS%3D%22success%22%0A%0A%7C%20dedup%20PROJECT_NAME%20WORKFLOW_NAME%20WORKFLOW_ID%0A%0A%7C%20eval%20workflow_duration%3Dround(strptime(WORKFLOW_STOPPED_AT%2C%20%22%25Y-%25m-%25d%20%25H%3A%25M%3A%25S.%253N%22)%20-%20strptime(WORKFLOW_FIRST_JOB_STARTED_AT%2C%20%22%25Y-%25m-%25d%20%25H%3A%25M%3A%25S.%253N%22)%2C%200)%0A%0A%7C%20bin%20span%3D1mon%20_time%0A%7C%20stats%20%0Asum(workflow_duration)%20AS%20sum_workflow_duration%0Adc(WORKFLOW_ID)%20AS%20dc_WORKFLOW_ID%0Aavg(workflow_duration)%20AS%20avg_workflow_duration%0Ap95(workflow_duration)%20AS%20p95_workflow_duration%0A%0Acount(eval(if(workflow_duration%3C%3D60%2C%201%2C%20null())))%20AS%20workflow_count_1_min%0Acount(eval(if(workflow_duration%3E60%20and%20workflow_duration%3C%3D180%2C%201%2C%20null())))%20AS%20workflow_count_3_min%0Acount(eval(if(workflow_duration%3E180%20and%20workflow_duration%3C%3D360%2C%201%2C%20null())))%20AS%20workflow_count_6_min%0Acount(eval(if(workflow_duration%3E360%2C%201%2C%20null())))%20AS%20workflow_count_above_6_min%0A%0Amax(workflow_duration)%20AS%20max_workflow_duration%0Amin(workflow_duration)%20AS%20min_workflow_duration%0A%60%60%60%0Astdev(workflow_duration)%20AS%20std_workflow_duration%0Ap50(workflow_duration)%20AS%20p50_workflow_duration%0Ap75(workflow_duration)%20AS%20p75_workflow_duration%20%0A%60%60%60%0Aby%20_time%20PROJECT_NAME%20WORKFLOW_NAME%0A%0A%7C%20eval%20%0Aavg_workflow_duration%3Dround(avg_workflow_duration%2C%200)%2C%0A%60%60%60%0Aavg_workflow_duration_calculated%3Dround(sum_workflow_duration%2Fdc_WORKFLOW_ID%2C%200)%2C%0A%60%60%60%0Amax_workflow_duration%3Dround(max_workflow_duration%2C%200)%2C%0Amin_workflow_duration%3Dround(min_workflow_duration%2C%200)%2C%0Astd_workflow_duration%3Dround(std_workflow_duration%2C%200)%2C%0Ap95_workflow_duration%3Dround(p95_workflow_duration%2C%200)%0A%60%60%60%2C%0Ap50_workflow_duration%3Dround(p50_workflow_duration%2C%200)%2C%0Ap75_workflow_duration%3Dround(p75_workflow_duration%2C%200)%0A%60%60%60%0A%7C%20sort%20-%20_time%20avg_workflow_duration&display.page.search.mode=fast&dispatch.sample_ratio=1&workload_pool=&earliest=-24h%40h&latest=now&display.page.search.tab=statistics&display.general.type=statistics&display.prefs.statistics.count=100&sid=1755155957.48&display.statistics.format.0=color&display.statistics.format.0.scale=minMidMax&display.statistics.format.0.colorPalette=minMidMax&display.statistics.format.0.colorPalette.minColor=%23FFFFFF&display.statistics.format.0.colorPalette.maxColor=%23D41F1F&display.statistics.format.0.field=avg_workflow_duration), the image build pipeline for expert-hub is not very efficient.

<br>

- p95_workflow_duration of sucessful expert-hub image build <span v-mark.circle.orange="1">was 859 seconds</span>

<br>

- avg_workflow_duration of sucessful expert-hub image build <span v-mark.circle.orange="1">was 471 seconds</span>

<br>

- [280 workflow](https://splunk.test10.pro/en-US/app/search/search?q=search%20index%3D%22ci%22%20sourcetype%3D%22circleci_metric%22%0A%0Aearliest%3D%2201%2F01%2F2025%3A00%3A00%3A00%22%20latest%3D%2208%2F01%2F2025%3A00%3A00%3A00%22%0A%0APROJECT_NAME%3D%22expert-hub%22%0A%20%20%20%20%0A%7C%20bin%20span%3D1mon%20_time%0A%7C%20stats%0Adc(WORKFLOW_ID)%20AS%20dc_WORKFLOW_ID%0Asum(TOTAL_CREDITS)%20AS%20TOTAL_CREDITS%0Asum(DLC_CREDITS)%20AS%20DLC_CREDITS%0Aby%20_time%0A%0A%7C%20eval%20%0Aestimated_total_cost%3Dround(TOTAL_CREDITS%2F100*0.06%2C%200)%2C%0Aestimated_DLC_cost%3Dround(DLC_CREDITS%2F100*0.06%2C%200)%0A%0A%7C%20table%20_time%20dc_WORKFLOW_ID%20TOTAL_CREDITS%20estimated_total_cost%20DLC_CREDITS%20estimated_DLC_cost%0A%0A%60%60%60%0A%7C%20bin%20span%3D1w%20_time%0A%7C%20stats%0Acount%20as%20job_count%0Asum(TOTAL_CREDITS)%20AS%20TOTAL_CREDITS%0Asum(COMPUTE_CREDITS)%20AS%20COMPUTE_CREDITS%0Asum(DLC_CREDITS)%20AS%20DLC_CREDITS%0Asum(USER_CREDITS)%20AS%20USER_CREDITS%0A%0Aby%20_time%0A%0A%7C%20eval%20%0Aestimated_total_cost%3Dround(TOTAL_CREDITS%2F100*0.06%2C%200)%2C%0Aestimated_compute_cost%3Dround(COMPUTE_CREDITS%2F100*0.06%2C%200)%2C%0Aestimated_user_cost%3Dround(USER_CREDITS%2F100*0.06%2C%200)%2C%0Aestimated_DLC_cost%3Dround(DLC_CREDITS%2F100*0.06%2C%200)%0A%7C%20addcoltotals%20*_CREDITS%20job_count%0A%60%60%60&display.page.search.mode=fast&dispatch.sample_ratio=1&workload_pool=&earliest=-24h%40h&latest=now&display.page.search.tab=statistics&display.general.type=statistics&display.statistics.totalsRow=1&display.statistics.wrap=0&display.visualizations.trellis.enabled=1&display.visualizations.charting.legend.placement=none&display.visualizations.charting.axisTitleX.visibility=collapsed&display.visualizations.charting.axisTitleY.visibility=collapsed&display.visualizations.charting.axisTitleY2.visibility=collapsed&display.visualizations.charting.chart.overlayFields=&display.visualizations.charting.layout.splitSeries=1&sid=1755157488.74) triggers <span v-mark.circle.orange="1">costs 125$ total</span>, including 41$ CircleCI docker_layer_caching cost

<br>

---
level: 2
---

# How to improve it?

Step 1: problem analysis

- Pick up one [pipeline execution](https://app.circleci.com/pipelines/github/third-bridge/expert-hub/1412) to check the behaviour

  <div v-click="1">
  - Kaden spent <span text-red>10m 17s</span> waiting for this image built
  </div>

- Check the [definitions](https://github.com/third-bridge/expert-hub/blob/5aa1cf30e1bc2c446473b422a92346c884a4580f/.circleci/config.yml#L1-L213) from source

  <div v-click="2">

  - <span text-red>213</span> lines of ".circleci/config.yml"

  - <span text-red>203</span> lines of "makefile"

  - <span text-red>123</span> lines of "Dockerfile"

  </div>
