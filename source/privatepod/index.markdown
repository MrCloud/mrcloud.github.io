---
layout: page
title: "privatepod"
date: 2014-06-11 22:51
comments: true
sharing: true
footer: true
---

#Create a custom CocoaPod

Requires the following:
  - Xcode
  - CocoaPods Installed with the private repo configured
  - A git repository for your library
Run:
```bash
pod lib create [PodName]
```

- Create a ```Demo``` Xcode project for the pod in the ```Example``` folder
- Duplicate your library files to the ```Classes``` folder
- Commit and push to your Library repository:
```bash
git add .
git commit -m "Initial Import"
git tag [your version number here](must match with the version in the VERSION file)
```
- Update the ```[PodName].podspec``` file according to your project info
Here is an example of working podspec file
```ruby
Pod::Spec.new do |s|
  s.name             = "DummyPod"
  s.version          = File.read('VERSION')
  s.summary          = "An example of pod named DummyPod."
  s.description      = <<-DESC
                       A Cocoapod created as an example for documentation purpose.
                       DESC
  s.homepage         = "https://github.com/MrCloud/DummyKit"
  # s.screenshots      = ""
  s.license          = 'MIT'
  s.author           = { "Florian PETIT" => "florianp37@me.com" }
  s.source           = { :git => "https://github.com/MrCloud/DummyKit.git", :tag => s.version.to_s }
  # s.social_media_url = ''

  # s.platform     = :ios, '7.0'
  # s.ios.deployment_target = '7.0'
  # s.osx.deployment_target = '10.9'
  s.requires_arc = true

  s.source_files = 'Classes/**/*.{h,m}'
  # s.resources = 'Assets/*.png'

  # s.ios.exclude_files = 'Classes/osx'
  # s.osx.exclude_files = 'Classes/ios'
  # s.public_header_files = 'Classes/**/*.h'
  # s.frameworks = 'SomeFramework', 'AnotherFramework'
  # s.dependency 'JSONKit', '~> 1.4'
end
```

- Run ```pod lib lint .```until you get no warning/error
You may have an error if you have never pushed files to your library repo
- Commit and push to your Library repository:
```bash
git add .
git commit -m "Initial Import"
git tag [your version number here](must match with the version in the podspec file)
```

- Finally deploy to the central repo:
```bash
pod repo push REPO_NAME [PodName].podspec
```

---
Optional Steps
- cd into the ```Example``` folder (eventually edit the ```Podfile```) then run:
```bash
pod install
```
- Now open your ```.xcworkspace``` and update your demo project or your development pods

---

Update an existing pod:
- Make your changes to your library
- Update your ```.podspec``` file
- Commit and push to your Library repository:
```bash
git add .
git commit -m "Initial Import"
git tag [your version number here](must match with the version in the VERSION file)
```

---
Resources:

http://guides.cocoapods.org/making/making-a-cocoapod.html

http://code.dblock.org/your-first-cocoapod

http://guides.cocoapods.org/making/private-cocoapods.html
