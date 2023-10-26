# Side-loading .NET Core agent

This article describes how to run your application in Docker without modifying the container image. It should not be used for Kubernetes when the [Agent Operator](https://docs.contrastsecurity.com/en/install-agent-operator.html) is preferred. A sample [docker-compose.yml](docker-compose.yml) file included for the 'Netflicks' demo application.

## 1. Download and Unzip the agent

1. Download the agent: `curl -L https://www.nuget.org/api/v2/package/Contrast.SensorsNetCore --output Contrast.SensorsNetCore.zip`
2. Unzip the folder: `unzip Contrast.SensorsNetCore.zip -d ./contrast`

## 2. Add the agent via a volume with docker-compose.yml file (or docker run)
```
    volumes:
      - ./contrast/:/opt/contrast/
```

## 3. Map your contrast_security.yaml file via your docker-compose.yml file (or docker run)
```
    volumes:
      - ./contrast_security.yaml:/etc/contrast/dotnet-core/contrast_security.yaml
```

## 4. Set the environment variables in your container via your docker-compose.yml file (or docker run)

```
    environment:
      - CORECLR_PROFILER_PATH_64=/opt/contrast/contentFiles/any/netstandard2.0/contrast/runtimes/linux-x64/native/ContrastProfiler.so
      - CORECLR_PROFILER={8B2CE134-0948-48CA-A4B2-80DDAD9F5791}
      - CORECLR_ENABLE_PROFILING=1
      - CONTRAST_CORECLR_LOGS_DIRECTORY=/tmp/contrast/
      - CONTRAST__APPLICATION__NAME=<your application name>
      - CONTRAST__SERVER__ENVIRONMENT=<dev, qa or production>
```

## Run `docker-compose up` (or docker run)

You should see contrast logs in the output:

```
sideload-dotnet-core-web-1       |            ___                             
sideload-dotnet-core-web-1       |        _.-|   |          |\__/,|   (`\     Contrast .NET Core Agent 4.1.6.0
sideload-dotnet-core-web-1       |       [   |   |          |o o  |__ _) )    
sideload-dotnet-core-web-1       |        `-.|___|        _.( T   )  `  /     Contrast UI: https://eval.contrastsecurity.com
sideload-dotnet-core-web-1       |         .--'-`-.     _((_ `^--' /_<  \     Mode:        Assess
sideload-dotnet-core-web-1       |       .+|______|__.-||__)`-'(((/  (((/     
sideload-dotnet-core-web-1       | 
```