---
layout: post
title: "Highlight Build Error"
date: 2015-03-25 9:24
category: Android
tags: android tools
---
{% include JB/setup %}

Highlight Build Error

------

    function mm() {
        type mm 1>/dev/null 2>&1 && mm $@ 2>&1 | sed -e "s/.*warning:.*/\x1b[31m&\x1b[m/g" -e "s/.*error:.*/\x1b[35m&\x1b[m/g"
    }

    function hl_build() {
        ls ./build.sh 1>/dev/null 2>&1 && ./build.sh $@ 2>&1 | sed -e "s/.*warning:.*/\x1b[31m&\x1b[m/g" -e "s/.*error:.*/\x1b[35m&\x1b[m/g"
    }

Could define below in ~/.bashrc
