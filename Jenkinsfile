#!groovy
@Library('Jenkins_SharedLibrary')

def tools = new org.devops.tools()



// Pipeline
pipeline {
	// 指定在哪个节点上执行pipeline
	agent any
	// 获取自动安装或者手动安装的环境变量00999
	tools {
		gradle "GRADLE"
	}

	// 指定运行的选项（可选）
	options {
		timestamps()	// 日志会有时间
		skipDefaultCheckout()	// 删除隐式checkout scm语句
		disableConcurrentBuilds()	//禁止并行
		timeout(time:1,unit:'HOURS') //设置流水线超时时间
	}

	// 构建阶段
	stages {
		// 下载代码
		stage("GetCode"){
			// 步骤
			steps{
				// 设置步骤超时时间
				timeout(time:5,unit:'MINUTES'){
					script{
						println("获取代码")
                        checkout([$class: 'GitSCM', branches: [[name: "${branchName}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'gitee', url: "${srcUrl}"]]])
					}
				}
			}
		}
		stage("Build"){
			steps{
				timeout(time:20,unit:'MINUTES'){
					script{
						println("代码打包")
						sh "gradle build bootJar --debug --info"
					}
				}
			}
		}
		stage("CodeScanner"){
			steps{
				timeout(time:30,unit:'MINUTES'){
					script{
					    sh "echo 代码检测"
					}
				}
			}
		}
        stage("ScanStatus"){
			steps{
				timeout(time:30,unit:"MINUTES"){
                    script{
                        // 获取扫描状态
                        sh "echo Scan"
						}
                    }
        		}
			}
		}
	}
}

// 构建后的操作
post {
    always {
        script{
            println("always:不论构建成功与否都会执行")
        }
    }
    success {
        script{
            println("success:只有构建成功才会执行")
            currentBuild.description += "\n构建成功！"
            dingmes.SendDingTalk("构建成功 ✅")
			sh "ansible all -m shell -a '/home/'"
        }
    }
    failure {
        script{
            println("failure:只有构建失败才会执行")
            currentBuild.description += "\n构建失败!"
            //sendEmail.SendEmail("构建失败",toEmailUser)
            dingmes.SendDingTalk("构建失败 ❌")

        }
    }
    aborted {
        script{
            println("aborted:只有取消构建才会执行")
            currentBuild.description += "\n构建取消!"
            //sendEmail.SendEmail("取消构建",toEmailUser)
            dingmes.SendDingTalk("构建失败 ❌","暂停或中断")
        }
    }
}
