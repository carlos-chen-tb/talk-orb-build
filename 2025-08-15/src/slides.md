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

# Talk orb-build <span class="text-size-6">(v0.1)</span>

Build container images faster and more cost-effectively

<div class="pt-12">
  <span class="text-s px-0 py-0 rounded" hover="bg-white bg-opacity-10">
    Carlos Chen / Matus Kacmar
  </span>
  <span class="text-xs rounded cursor-pointer" hover="bg-white bg-opacity-10">
    @2025-08-15
  </span>
</div>

<div w-full absolute bottom-0 left-0 flex items-center transform="translate-x--10 translate-y--10">
  <div w-full flex items-center justify-end gap-4>
    <img src="/ThirdBridge_Horizontal_Lockup_Blue_Dark_RGB_1200x185.png" h-6 w-40 >
  </div>
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

<span v-mark.circle.orange="4">[Reusable CircleCI command or job](https://github.com/search?q=org%3Athird-bridge+path%3A%2F%5E%5C.circleci%5C%2F.*%5C.yml%24%2F+%2F%28thirdbridge%7Cserverless%29%5C%2F.*%40%2F&type=code&p=2)</span> to use in your [.circleci/config.yml](https://github.com/third-bridge/expert-hub/blob/edc42fbabdf65c3af3fe4cb4338e6d31fdea8720/.circleci/build.yml)

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

# How does it look?

[In CircleCI Web UI](https://app.circleci.com/pipelines/github/third-bridge/expert-hub/5187/workflows/2254137d-bbfd-4e9c-ba9f-79585c841ac2/jobs/6801)

<img src="/expert-hub_node-image-build_circleci_ui.png" w-full />

---

# How does it look?

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

- [280 workflow](https://splunk.test10.pro/en-US/app/search/search?q=search%20index%3D%22ci%22%20sourcetype%3D%22circleci_metric%22%0A%0Aearliest%3D%2201%2F01%2F2025%3A00%3A00%3A00%22%20latest%3D%2208%2F01%2F2025%3A00%3A00%3A00%22%0A%0APROJECT_NAME%3D%22expert-hub%22%0A%20%20%20%20%0A%7C%20bin%20span%3D1mon%20_time%0A%7C%20stats%0Adc(WORKFLOW_ID)%20AS%20dc_WORKFLOW_ID%0Asum(TOTAL_CREDITS)%20AS%20TOTAL_CREDITS%0Asum(DLC_CREDITS)%20AS%20DLC_CREDITS%0Asum(COMPUTE_CREDITS)%20AS%20COMPUTE_CREDITS%0Asum(USER_CREDITS)%20AS%20USER_CREDITS%0Aby%20_time%0A%0A%7C%20eval%20%0Aestimated_total_cost%3Dround(TOTAL_CREDITS%2F100*0.06%2C%200)%2C%0Aestimated_compute_cost%3Dround(COMPUTE_CREDITS%2F100*0.06%2C%200)%2C%0Aestimated_user_cost%3Dround(USER_CREDITS%2F100*0.06%2C%200)%2C%0Aestimated_DLC_cost%3Dround(DLC_CREDITS%2F100*0.06%2C%200)%0A%0A%7C%20table%20_time%20dc_WORKFLOW_ID%20TOTAL_CREDITS%20estimated_total_cost%20DLC_CREDITS%20estimated_DLC_cost%20COMPUTE_CREDITS%20estimated_compute_cost%20USER_CREDITS%20estimated_user_cost%0A%0A%60%60%60%0A%7C%20addcoltotals%20*_CREDITS%20job_count%0A%60%60%60&display.page.search.mode=fast&dispatch.sample_ratio=1&workload_pool=&earliest=-24h%40h&latest=now&display.page.search.tab=statistics&display.general.type=statistics&display.statistics.totalsRow=1&display.statistics.wrap=0&display.visualizations.trellis.enabled=1&display.visualizations.charting.legend.placement=none&display.visualizations.charting.axisTitleX.visibility=collapsed&display.visualizations.charting.axisTitleY.visibility=collapsed&display.visualizations.charting.axisTitleY2.visibility=collapsed&display.visualizations.charting.chart.overlayFields=&display.visualizations.charting.layout.splitSeries=1&sid=1755242983.217&display.statistics.format.0=color&display.statistics.format.0.scale=minMidMax&display.statistics.format.0.colorPalette=minMidMax&display.statistics.format.0.colorPalette.minColor=%23FFFFFF&display.statistics.format.0.colorPalette.maxColor=%23D41F1F&display.statistics.format.0.field=estimated_compute_cost&display.statistics.format.1=color&display.statistics.format.1.scale=minMidMax&display.statistics.format.1.colorPalette=minMidMax&display.statistics.format.1.colorPalette.minColor=%23FFFFFF&display.statistics.format.1.colorPalette.maxColor=%23D41F1F&display.statistics.format.1.field=estimated_DLC_cost) triggers <span v-mark.circle.orange="1">costs 125$ total</span>, including 41$ CircleCI docker_layer_caching cost

<br>

---
level: 2
---

# How to improve it?

Problem analysis

- Pick up one [pipeline execution](https://app.circleci.com/pipelines/github/third-bridge/expert-hub/1412) to check the behaviour

  <div v-click="1">
  - Kaden spent <span text-red>10m 17s</span> waiting for this image built
  </div>

<br>

<div v-click="2">

- Check the [definitions](https://github.com/third-bridge/expert-hub/blob/5aa1cf30e1bc2c446473b422a92346c884a4580f/.circleci/config.yml#L1-L213) from source

  <div v-click="3">

  - <span text-red>213</span> lines of ".circleci/config.yml"

  - <span text-red>203</span> lines of "makefile"

  - <span text-red>123</span> lines of "Dockerfile"

  </div>

</div>
---
level: 2
---

# How to improve it?

Clarify requirements

<br>

- yarn/pnpm/npm install

- eslint

- jest

- next build, nest build

- image build

---
level: 2
layout: center
class: py-10
---

# How to improve it?

<div mt-6 />

<div class="mb-4">
  <div text-xl font-bold text-orange-300 flex items-center>
    <div i-carbon:flow mr-3 />Possible improvements
  </div>
</div>

<div grid grid-cols-4 gap-4>
  <div v-click="1" class="rounded-lg p-12 bg-red-900/20 text-center flex flex-col items-center gap-2">
    <div i-carbon:document text="[50px]" text-red-400 mb-2 />
    <div font-bold text-sm text-nowrap>Cache</div>
    <div text-left text-xs text-nowrap text-red-200 mt-1>
      <br>
      node_moduels <br><br>
      .next/cache <br><br>
      container registry cache <br>
      <br>
    </div>
  </div>

  <div v-click="2" class="rounded-lg p-12 bg-orange-900/20 text-center flex flex-col items-center gap-2">
    <div i-carbon:settings text="[50px]" text-orange-400 mb-2 />
    <div font-bold text-sm text-nowrap>Config</div>
    <div text-left text-xs text-nowrap text-orange-200 mt-1>
      <br>
      jest --onlyChanged ... <br><br>
      <div text-size-2 text-wrap text-red-250 mt-1>
      eslint $(git diff main...HEAD) <br>
      </div>
      <br>
      Next: output: 'standalone' <br>
    </div>
  </div>

  <div v-click="3" class="rounded-lg p-12 bg-amber-900/20 text-center flex flex-col items-center gap-2">
    <div i-carbon:chip text="[50px]" text-amber-400 mb-2 />
    <div font-bold text-sm text-nowrap>Dockerfile</div>
    <div text-left text-xs text-nowrap text-amber-200 mt-1>
      <div text-size-2 text-wrap text-red-250 mt-1>
      <br>
      move base docker stage steps into CircleCI steps <br>
      </div>
    <br>
    whitelist dockerignore
    </div>
  </div>

  <div v-click="4" class="rounded-lg p-12 bg-yellow-900/20 text-center flex flex-col items-center gap-2">
    <div i-carbon:play text="[50px]" text-yellow-400 mb-2 />
    <div font-bold text-sm text-nowrap>Execution</div>
    <div text-xs text-wrap text-yellow-200 mt-1>
          <br>
          <br>
          CircleCI machine executor seems a bit quicker
    </div>
  </div>

  <!-- <div v-click="6" class="rounded-lg p-12 bg-lime-900/20 text-center flex flex-col items-center gap-2">
    <div i-carbon:port-output text="[50px]" text-lime-400 mb-2 />
    <div font-bold text-sm text-nowrap>Decode</div>
    <div text-xs text-nowrap text-lime-200 mt-1>解码输出</div>
  </div> -->
</div>

  <div v-click="5" mt-4 class="bg-white/10 rounded-lg p-4">
  <div text-lg font-bold mb-4 text-neutral-200>Show me code</div>
  <div flex flex-wrap gap-3>
    <div class="px-3 py-2 bg-blue-800/30 rounded-full text-sm">
      <a href="https://github.com/third-bridge/expert-hub/blob/541d54d51c2f01b425c0ca7755f09372157dca0d/.circleci/build.yml#L1">Implementation</a>
    </div>
    <div v-click="6" class="px-3 py-2 bg-blue-800/30 rounded-full text-sm">
      <a href="https://github.com/third-bridge/orb-build">Make it reusable</a>
    </div>
  </div>
</div>

---
level: 2
layout: center
class: py-10
---

# Some useful [parameters](https://circleci.com/developer/orbs/orb/thirdbridge/build#jobs-node_image_build)

<div mt-6 />

<div class="mb-4">
  <div text-xl font-bold text-orange-300 flex items-center>
  <div mr-3 />Recommend to install VSCode CircleCI extention
  </div>
</div>

<div grid grid-cols-4 gap-4>
  <div v-click="1" class="rounded-lg p-12 bg-red-900/20 text-center flex flex-col items-center gap-2">
    <div font-bold text-sm text-nowrap>nodejs</div>
    <div text-left text-xs text-nowrap text-red-200 mt-1>
      <br>
      package_json_directory: . <br>
      node_version: 22.18.0 <br>
      package_manager: yarn <br>
      package_manager_version: 1  <br><br>
      install_command: "" <br>
      build_command: "" <br>
      lint_command: "" <br>
      test_command: "" <br>
      skip_install: false <br>
      skip_lint: false <br>
      skip_test: false <br>
      skip_build: false <br>
      circleci_cache_version: "v1" <br>
      next_cache_directory: ./.next <br>
      ...
      <br>
    </div>
  </div>

  <div v-click="2" class="rounded-lg p-12 bg-orange-900/20 text-center flex flex-col items-center gap-2">
    <div font-bold text-sm text-nowrap>image_build</div>
    <div text-left text-xs text-nowrap text-orange-200 mt-1>
      <br>
      dockerfile: Dockerfile.prod <br><br>
      repo: "" <br><br>
      tag: "" <br><br>
      extra_build_args: "" <br><br>
      ...
      <br>
    </div>
  </div>

  <div v-click="3" class="rounded-lg p-12 bg-amber-900/20 text-center flex flex-col items-center gap-2">
    <div font-bold text-sm text-nowrap>notify</div>
    <div text-left text-xs text-nowrap text-amber-200 mt-1>
      <div text-xs text-wrap text-red-250 mt-1>
      <br>
      enable_slack_notify: true <br><br>
      channel: artemis-notify <br><br>
      </div>
    <br>
    </div>
  </div>

  <div v-click="4" class="rounded-lg p-12 bg-yellow-900/20 text-center flex flex-col items-center gap-2">
    <div font-bold text-sm text-nowrap>others</div>
    <div text-left text-xs text-wrap text-yellow-200 mt-1>
      <br>
      debug: true <br><br>
      executor: "" <br><br>
      repo: "" <br><br>
      tag: "" <br><br>
      extra_build_args: "" <br><br>
      ...
    </div>
  </div>

  <!-- <div v-click="6" class="rounded-lg p-12 bg-lime-900/20 text-center flex flex-col items-center gap-2">
    <div i-carbon:port-output text="[50px]" text-lime-400 mb-2 />
    <div font-bold text-sm text-nowrap>Decode</div>
    <div text-xs text-nowrap text-lime-200 mt-1>解码输出</div>
  </div> -->
</div>

---

# More examples of using orb-build

- orb-build/image_build

  - [intranet-v2 test containers](https://github.com/third-bridge/intranet-v2/pull/2038/files)<span v-click="1">, </span><span v-click="1" text-orange>1300 credits</span> <span v-click="1" text-orange>-></span> <span v-click="1" text-green>90 credits</span>

<br>
<div v-click="2">

- orb-build/node_image_build

  - Randomly selected [transcript-portal](https://github.com/third-bridge/transcript-portal/compare/CLD-3185-evaluate-image-build-orb-job-performance-in-more-service-repos) to test

    - p95_JOB_RUN_SECONDS of build-test-push is <span text-orange>566 seconds</span>, could reduce to <span text-green>[266 seconds](https://wearethirdbridge.slack.com/archives/C08TVMX0MLP/p1753956323140299?thread_ts=1753956075.362639&cid=C08TVMX0MLP)</span>

    - DLC credits will be <span text-green>0</span>

- <span text-orange>! It's in v0.1.13, we need more review from both DevSecOps internal and wider Engineering</span>

</div>

---
layout: center
class: text-center
---

# Thank you!
