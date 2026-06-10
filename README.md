# Strava Activity Cleaner

A small browser-console script for deleting strength-training activities from Strava activity pages.
I use Huawei Band and my Huawei Health app unfortunately syncs all of my workouts. This is troublesome for me since logging your strength training in Strava serves little purpose.

## What it does

The script scans the current Strava activity page, finds rows that appear to be strength-training activities, highlights them for reviewo and can delete them after confirmation.

## Important warnings

- Use at your own risk.
- Review highlighted activities before deletion.
- Do not run this on an unfiltered general activity page unless you are sure the matching logic is correct.
- Do not include cookies, CSRF tokens, activity IDs, or private browser headers in this repository.
- This is not an official Strava tool and is not affiliated with Strava.

## AI assistance disclosure

This script was developed with assistance from ChatGPT. I reviewed and tested the code before publishing it.
