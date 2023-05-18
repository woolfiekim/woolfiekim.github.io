---
layout: single
title:  "[Jenkins] java.lang.NoClassDefFoundError: Could not initialize class sun.nio.fs.LinuxNativeDispatcher"
categories: 
  - etc  #카테고리
tag: [jenkins, 빌드, 젠킨스] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

오늘 회사에서 가상서버의 서버시간을 바꿔보려고 시도했다.

리눅스(CentOS)에 설치된 패키지(yum)을 업데이트를 해줬다.
(참고로, 업데이트는 되었는데 미국시간에서 한국서버시간으로 바꾸는 것은 어려워서 포기했다.)
(yum은 맥의 brew와 비슷한 거라고 생각하면 된다.)

그 뒤에 한 프로젝트의 코드를 수정하고 젠킨스를 통해 빌드를 실패했다.

## 1. 에러메세지

```shell
FATAL: Could not initialize class sun.nio.fs.LinuxNativeDispatcher
java.lang.NoClassDefFoundError: Could not initialize class sun.nio.fs.LinuxNativeDispatcher
	at java.base/sun.nio.fs.LinuxFileSystem.getMountEntries(LinuxFileSystem.java:81)
	at java.base/sun.nio.fs.LinuxFileStore.findMountEntry(LinuxFileStore.java:75)
	at java.base/sun.nio.fs.UnixFileStore.<init>(UnixFileStore.java:69)
	at java.base/sun.nio.fs.LinuxFileStore.<init>(LinuxFileStore.java:49)
	at java.base/sun.nio.fs.LinuxFileSystemProvider.getFileStore(LinuxFileSystemProvider.java:51)
	at java.base/sun.nio.fs.LinuxFileSystemProvider.getFileStore(LinuxFileSystemProvider.java:39)
	at java.base/sun.nio.fs.UnixFileSystemProvider.getFileStore(UnixFileSystemProvider.java:373)
	at java.base/java.nio.file.Files.getFileStore(Files.java:1488)
	at org.eclipse.jgit.util.FS$FileStoreAttributes.getFileStoreAttributes(FS.java:362)
	at org.eclipse.jgit.util.FS$FileStoreAttributes.get(FS.java:346)
	at org.eclipse.jgit.util.FS.getFileStoreAttributes(FS.java:913)
	at org.eclipse.jgit.internal.storage.file.FileSnapshot.<init>(FileSnapshot.java:226)
	at org.eclipse.jgit.internal.storage.file.FileSnapshot.<init>(FileSnapshot.java:207)
	at org.eclipse.jgit.internal.storage.file.FileSnapshot.save(FileSnapshot.java:104)
	at org.eclipse.jgit.internal.storage.file.FileRepository.<init>(FileRepository.java:205)
	at org.eclipse.jgit.lib.BaseRepositoryBuilder.build(BaseRepositoryBuilder.java:625)
	at org.jenkinsci.plugins.gitclient.CliGitAPIImpl.getRepository(CliGitAPIImpl.java:3407)
	at hudson.plugins.git.GitAPI.getRepository(GitAPI.java:288)
	at org.jenkinsci.plugins.gitclient.AbstractGitAPIImpl.withRepository(AbstractGitAPIImpl.java:28)
	at org.jenkinsci.plugins.gitclient.CliGitAPIImpl.withRepository(CliGitAPIImpl.java:84)
	at hudson.plugins.git.GitSCM.printCommitMessageToLog(GitSCM.java:1386)
	at hudson.plugins.git.GitSCM.checkout(GitSCM.java:1352)
	at hudson.scm.SCM.checkout(SCM.java:540)
	at hudson.model.AbstractProject.checkout(AbstractProject.java:1241)
	at hudson.model.AbstractBuild$AbstractBuildExecution.defaultCheckout(AbstractBuild.java:649)
	at jenkins.scm.SCMCheckoutStrategy.checkout(SCMCheckoutStrategy.java:85)
	at hudson.model.AbstractBuild$AbstractBuildExecution.run(AbstractBuild.java:521)
	at hudson.model.Run.execute(Run.java:1900)
	at hudson.model.FreeStyleBuild.run(FreeStyleBuild.java:44)
	at hudson.model.ResourceController.execute(ResourceController.java:107)
	at hudson.model.Executor.run(Executor.java:449)
[DeployPublisher][INFO] Build failed, project not deployed
[WARN] Not a WorkflowRun, onCompleted won't be propagated to listeners
Finished: FAILURE
```

이게 젠킨스에서 나온 오류이다. 

대충 구글링을 한 결과 JDK, 자바를 다시 재설치를 하라는 것이였다. 

아...안된다 이놈들아!

그런 거추장스런 일을 하기 싫다.

## 2. 해결

그러다가 어떤 외국인의 답변을 보게 되었다.

![answer](/assets/images/2023/02/14/answer.png)

한 번 해보기로 했다.

프로젝트가 빌드 되어있는 장비에 들어가서 

```bash
systemctl restart jenkins
```

이 명령어를 입력하였다.

그리고 다시 젠킨스에 들어가서 빌드 버튼을 눌렀더니!

어예~~ 됬다~~~

![smile](/assets/images/2023/02/14/smile.jpg)