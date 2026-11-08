---
# Leave the homepage title empty to use the site title
title: ''
summary: ''
date: 2026-11-08
type: landing

sections:
  - block: resume-biography-3
    content:
      username: me
      text: |-
        I am a postdoctoral researcher at **LabTAU (INSERM, France)**, where I build numerical
        tools for **high-intensity focused ultrasound (HIFU)** therapy of prostate cancer:
        coupled thermoacoustic models, patient-specific simulations and their validation
        against post-treatment MRI.

        I came to ultrasound from **civil and structural engineering**. At the University of
        Granada I moved into biomechanics and completed a PhD on **dynamic elastography**,
        characterising viscoelastic soft media with torsional, longitudinal and shear waves.
        Along the way I co-founded the med-tech spin-off **Innitius**, wrote patents, designed
        prototypes and taught engineering mechanics.

        Today I work where physics, software and clinical needs meet. I am open to
        collaborations in **academia and industry**.
      headings:
        about: ''
        education: ''
        interests: ''
    design:
      background:
        gradient_mesh:
          enable: true
      name:
        size: md
      avatar:
        size: medium
        shape: circle

  - block: focus-areas
    id: explore
    content:
      title: Two ways to explore my work
      subtitle: Pick the view that fits what you are looking for
      items:
        - name: For researchers
          icon: hero/academic-cap
          description: >-
            Journal articles and conference talks on HIFU modelling, elastography,
            viscoelasticity and nonlinear wave propagation, plus funded research projects.
          topics: [HIFU, Elastography, Viscoelasticity, Thermoacoustics]
          metrics:
            - label: Journal articles
              value: '16'
            - label: Funded projects
              value: '7'
          cta:
            text: Publications & talks
            url: /publications/
        - name: For industry
          icon: hero/briefcase
          description: >-
            Experience, skills, software and IP: GPU-accelerated simulation (SimTAU),
            start-up co-founding (Innitius), patents, prototyping and data analysis in Python.
          topics: [Simulation software, Python, Prototyping, Patents]
          metrics:
            - label: Years in R&D
              value: '8+'
            - label: Start-up co-founded
              value: '1'
          cta:
            text: Industry profile
            url: /industry/
        - name: Notes & code
          icon: hero/code-bracket
          description: >-
            Short explanations of hard concepts with worked examples, and small Python
            scripts and snippets you can reuse.
          topics: [Python, Signal processing, Tutorials]
          cta:
            text: Read the notes
            url: /blog/
    design:
      layout: cards

  - block: collection
    id: papers
    content:
      title: Featured Publications
      text: '[See all publications →](/publications/)'
      filters:
        folders:
          - publications
        featured_only: true
    design:
      view: citation

  - block: collection
    id: talks
    content:
      title: Recent Talks
      text: '[All talks →](/events/)'
      count: 3
      filters:
        folders:
          - events
    design:
      view: talk-list

  - block: collection
    id: notes
    content:
      title: Notes & Snippets
      text: Short explanations of hard concepts and small, reusable pieces of code.
      count: 3
      filters:
        folders:
          - blog
    design:
      view: compact-card
---
