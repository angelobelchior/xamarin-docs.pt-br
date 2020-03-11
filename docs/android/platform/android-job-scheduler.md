---
title: Agendador de trabalhos do Android
description: Este guia discute como agendar o trabalho em segundo plano usando a API do Agendador de trabalhos do Android.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/19/2018
ms.openlocfilehash: 10d2ae6ac35f02d75ef6e04a0531ec3f5dafd668
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78292610"
---
# <a name="android-job-scheduler"></a>Agendador de trabalhos do Android

_Este guia discute como agendar o trabalho em segundo plano usando a API do Agendador de trabalho do Android, que está disponível em dispositivos Android que executam o Android 5,0 (API nível 21) e superior._

## <a name="overview"></a>Visão geral 

Uma das melhores maneiras de manter um aplicativo Android responsivo ao usuário é garantir que o trabalho complexo ou de longa execução seja executado em segundo plano. No entanto, é importante que o trabalho em segundo plano não afete negativamente a experiência do usuário com o dispositivo. 

Por exemplo, um trabalho em segundo plano pode sondar um site a cada três ou quatro minutos para consultar alterações em um determinado conjunto de dado. Isso parece benigno, mas teria um impacto desastroso na vida útil da bateria. O aplicativo ativará repetidamente o dispositivo, elevará a CPU a um estado de energia mais alto, ligará as rádios, fará as solicitações de rede e, em seguida, processando os resultados. Isso é pior porque o dispositivo não será imediatamente desligado e retornará ao estado ocioso de baixa energia. Um trabalho em segundo plano agendado incorretamente pode manter o dispositivo inadvertidamente em um estado com requisitos de energia excessivas e desnecessários. Essa atividade aparentemente inocente (sondando um site) tornará o dispositivo inutilizável em um período de tempo relativamente curto.

O Android fornece as seguintes APIs para ajudar na execução do trabalho em segundo plano, mas por si só eles não são suficientes para o agendamento inteligente de trabalhos. 

- Serviços de **[intenção](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; serviço de intenção são ótimos para executar o trabalho, no entanto, eles não fornecem como agendar o trabalho.
- O **[alarmmanager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; essas APIs só permitem que o trabalho seja agendado, mas não fornecem como realmente realizar o trabalho. Além disso, o Alarmmanager permite apenas restrições baseadas em tempo, o que significa gerar um alarme em um determinado momento ou depois de um determinado período de tempo decorrido. 
- Os **[receptores de difusão](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; um aplicativo Android podem configurar receptores de difusão para executar o trabalho em resposta a eventos ou tentativas de todo o sistema. No entanto, os receptores de difusão não fornecem nenhum controle sobre quando o trabalho deve ser executado. Além disso, as alterações no sistema operacional Android serão restritas quando os receptores de transmissão funcionarem ou os tipos de trabalho aos quais eles podem responder. 

Há dois recursos principais para executar com eficiência o trabalho em segundo plano (às vezes chamado de _trabalho em segundo plano_ ou um _trabalho_):

1. **Agendando o trabalho de forma inteligente** &ndash; é importante que, quando um aplicativo estiver trabalhando em segundo plano, ele faça isso como um bom cidadão. O ideal é que o aplicativo não exija que um trabalho seja executado. Em vez disso, o aplicativo deve especificar condições que devem ser atendidas para quando o trabalho puder ser executado e, em seguida, agendar esse trabalho com o sistema operacional que executará o trabalho quando as condições forem atendidas. Isso permite que o Android execute o trabalho para garantir a máxima eficiência no dispositivo. Por exemplo, as solicitações de rede podem ser executadas em lotes para executar tudo ao mesmo tempo para fazer o uso máximo da sobrecarga envolvida na rede.
2. **Encapsular o trabalho** &ndash; o código para executar o trabalho em segundo plano deve ser encapsulado em um componente discreto que pode ser executado independentemente da interface do usuário e será relativamente fácil reagendar se o trabalho não for concluído por algum motivo.

O Agendador de trabalhos do Android é uma estrutura interna ao sistema operacional Android que fornece uma API fluente para simplificar o agendamento do trabalho em segundo plano.  O Agendador de trabalhos do Android consiste nos seguintes tipos:

- O `Android.App.Job.JobScheduler` é um serviço de sistema que é usado para agendar, executar e, se necessário, cancelar, trabalhos em nome de um aplicativo Android.
- Um `Android.App.Job.JobService` é uma classe abstrata que deve ser estendida com a lógica que executará o trabalho no thread principal do aplicativo. Isso significa que a `JobService` é responsável por como o trabalho deve ser executado de forma assíncrona.
- Um objeto `Android.App.Job.JobInfo` mantém os critérios para guiar o Android quando o trabalho deve ser executado.

Para agendar o trabalho com o Agendador de trabalhos do Android, um aplicativo Xamarin. Android deve encapsular o código em uma classe que estende a classe `JobService`. `JobService` tem três métodos de ciclo de vida que podem ser chamados durante o tempo de vida do trabalho:

- **bool OnStartJob (parâmetros de JobParameters)** &ndash; esse método é chamado pelo `JobScheduler` para executar o trabalho e é executado no thread principal do aplicativo. É responsabilidade do `JobService` executar o trabalho de forma assíncrona e retornar `true` se houver trabalho restante ou `false` se o trabalho for feito.
    
    Quando o `JobScheduler` chama esse método, ele solicitará e manterá um wakelock do Android durante o trabalho. Quando o trabalho é concluído, é responsabilidade do `JobService` dizer ao `JobScheduler` desse fato chamar o método `JobFinished` (descrito a seguir).

- **JobFinished (parâmetros JobParameters, bool needsReschedule)** &ndash; esse método deve ser chamado pelo `JobService` para informar ao `JobScheduler` que o trabalho foi feito. Se `JobFinished` não for chamado, o `JobScheduler` não removerá o wakelock, causando drenagem desnecessária da bateria. 

- **bool OnStopJob (parâmetros de JobParameters)** &ndash; isso é chamado quando o trabalho é interrompido prematuramente pelo Android. Ele deve retornar `true` se o trabalho deve ser reagendado com base nos critérios de repetição (discutidos abaixo mais detalhadamente).

É possível especificar _restrições_ ou _gatilhos_ que controlarão quando um trabalho pode ou deve ser executado. Por exemplo, é possível restringir um trabalho para que ele seja executado somente quando o dispositivo estiver carregando ou para iniciar um trabalho quando uma imagem for executada.

Este guia discutirá detalhadamente como implementar uma classe de `JobService` e agendá-la com o `JobScheduler`.

## <a name="requirements"></a>Requisitos

O Agendador de trabalhos do Android requer a API do Android 21 (Android 5,0) ou superior. 

## <a name="using-the-android-job-scheduler"></a>Usando o Agendador de trabalhos do Android

Há três etapas para usar a API JobScheduler do Android:

1. Implemente um tipo JobService para encapsular o trabalho.
2. Use um objeto `JobInfo.Builder` para criar o objeto `JobInfo` que conterá os critérios para o `JobScheduler` executar o trabalho. 
3. Agende o trabalho usando `JobScheduler.Schedule`.

### <a name="implement-a-jobservice"></a>Implementar um JobService

Todo o trabalho executado pela biblioteca do Agendador de trabalhos do Android deve ser feito em um tipo que estenda a classe abstrata `Android.App.Job.JobService`. Criar um `JobService` é muito semelhante a criar um `Service` com a estrutura do Android: 

1. Estenda a classe `JobService`.
2. Decorar a subclasse com a `ServiceAttribute` e defina o parâmetro `Name` como uma cadeia de caracteres que seja composta pelo nome do pacote e pelo nome da classe (consulte o exemplo a seguir).
3. Defina a propriedade `Permission` no `ServiceAttribute` como a cadeia de caracteres `android.permission.BIND_JOB_SERVICE`.
4. Substitua o método `OnStartJob`, adicionando o código para executar o trabalho. O Android invocará esse método no thread principal do aplicativo para executar o trabalho. Trabalho que levará mais tempo que alguns milissegundos devem ser executados em um thread para evitar o bloqueio do aplicativo.
5. Quando o trabalho é concluído, o `JobService` deve chamar o método `JobFinished`. Esse método é como `JobService` informa ao `JobScheduler` que o trabalho é feito. A falha ao chamar `JobFinished` resultará na `JobService` de colocar demandas desnecessárias no dispositivo, reduzindo a vida útil da bateria. 
6. É uma boa ideia também substituir o método `OnStopJob`. Esse método é chamado pelo Android quando o trabalho está sendo desligado antes de ser concluído e fornece ao `JobService` uma oportunidade de descartar corretamente todos os recursos. Esse método deve retornar `true` se for necessário reagendar o trabalho ou `false` se não for desejável executar novamente o trabalho.

O código a seguir é um exemplo da `JobService` mais simples para um aplicativo, usando a TPL para realizar um trabalho de forma assíncrona:

```csharp
[Service(Name = "com.xamarin.samples.downloadscheduler.DownloadJob", 
         Permission = "android.permission.BIND_JOB_SERVICE")]
public class DownloadJob : JobService
{
    public override bool OnStartJob(JobParameters jobParams)
    {            
        Task.Run(() =>
        {
            // Work is happening asynchronously
                      
            // Have to tell the JobScheduler the work is done. 
            JobFinished(jobParams, false);
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(JobParameters jobParams)
    {
        // we don't want to reschedule the job if it is stopped or cancelled.
        return false; 
    }
}
```

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>Criando um JobInfo para agendar um trabalho

Os aplicativos Xamarin. Android não instanciam um `JobService` diretamente, em vez disso, eles passarão um objeto `JobInfo` para o `JobScheduler`. O `JobScheduler` criará uma instância do objeto de `JobService` solicitado, agendando e executando o `JobService` de acordo com os metadados no `JobInfo`. Um objeto `JobInfo` deve conter as seguintes informações:

- **JobId** &ndash; é um valor `int` que é usado para identificar um trabalho para o `JobScheduler`. A reutilização desse valor atualizará todos os trabalhos existentes. O valor deve ser exclusivo para o aplicativo. 
- **JobService** &ndash; esse parâmetro é um `ComponentName` que identifica explicitamente o tipo que o `JobScheduler` deve usar para executar um trabalho. 

Esse método de extensão demonstra como criar um `JobInfo.Builder` com um `Context`Android, como uma atividade:

```csharp
public static class JobSchedulerHelpers
{
    public static JobInfo.Builder CreateJobBuilderUsingJobId<T>(this Context context, int jobId) where T:JobService
    {
        var javaClass = Java.Lang.Class.FromType(typeof(T));
        var componentName = new ComponentName(context, javaClass);
        return new JobInfo.Builder(jobId, componentName);
    }
}

// Sample usage - creates a JobBuilder for a DownloadJob and sets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creates a JobInfo object.
```

Um recurso poderoso do Agendador de trabalhos do Android é a capacidade de controlar quando um trabalho é executado ou sob quais condições um trabalho pode ser executado. A tabela a seguir descreve alguns dos métodos em `JobInfo.Builder` que permitem que um aplicativo influencie quando um trabalho pode ser executado:  

|  Método | DESCRIÇÃO   |
|---|---|
| `SetMinimumLatency`  | Especifica que um atraso (em milissegundos) deve ser observado antes de um trabalho ser executado. |
| `SetOverridingDeadline`  | Declara que o trabalho deve ser executado antes dessa hora (em milissegundos) ter decorrido. |
| `SetRequiredNetworkType`  | Especifica os requisitos de rede para um trabalho. |
| `SetRequiresBatteryNotLow` | O trabalho só pode ser executado quando o dispositivo não estiver exibindo um aviso de "bateria fraca" ao usuário. |
| `SetRequiresCharging` | O trabalho só pode ser executado quando a bateria está carregando. |
| `SetDeviceIdle` | O trabalho será executado quando o dispositivo estiver ocupado. |
| `SetPeriodic` | Especifica que o trabalho deve ser executado regularmente. |
| `SetPersisted` | O trabalho deve ser perisisável entre as reinicializações do dispositivo. | 

O `SetBackoffCriteria` fornece algumas diretrizes sobre quanto tempo o `JobScheduler` deve aguardar antes de tentar executar um trabalho novamente. Há duas partes para os critérios de retirada: um atraso em milissegundos (valor padrão de 30 segundos) e o tipo de retirada que deve ser usado (às vezes chamado de _política de retirada_ ou a _política de repetição_). As duas políticas são encapsuladas na enumeração `Android.App.Job.BackoffPolicy`:

- `BackoffPolicy.Exponential` &ndash; uma política de retirada exponencial aumentará o valor inicial da retirada exponencialmente após cada falha. Na primeira vez que um trabalho falhar, a biblioteca aguardará o intervalo inicial especificado antes de reagendar o trabalho – exemplo 30 segundos. Na segunda vez em que o trabalho falhar, a biblioteca aguardará pelo menos 60 segundos antes de tentar executar o trabalho. Após a terceira tentativa com falha, a biblioteca aguardará 120 segundos e assim por diante. Esse é o valor padrão.
- `BackoffPolicy.Linear` &ndash; essa estratégia é uma retirada linear de que o trabalho deve ser reagendado para ser executado em intervalos definidos (até que tenha sucesso). A retirada linear é mais adequada para o trabalho que deve ser concluído assim que possível ou para problemas que serão resolvidos rapidamente. 

Para obter mais detalhes sobre como criar um objeto `JobInfo`, leia [a documentação do Google para a classe `JobInfo.Builder`](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html).

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>Passando parâmetros para um trabalho por meio de JobInfo

Os parâmetros são passados para um trabalho criando um `PersistableBundle` que é passado junto com o método `Job.Builder.SetExtras`:

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

O `PersistableBundle` é acessado a partir da propriedade `Android.App.Job.JobParameters.Extras` no método `OnStartJob` de um `JobService`:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>Agendando um trabalho

Para agendar um trabalho, um aplicativo Xamarin. Android receberá uma referência para o serviço do sistema `JobScheduler` e chamará o método `JobScheduler.Schedule` com o objeto `JobInfo` que foi criado na etapa anterior. `JobScheduler.Schedule` retornará imediatamente com um dos dois valores inteiros:

- **JobScheduler. ResultSuccess** &ndash; o trabalho foi agendado com êxito. 
- **JobScheduler. ResultFailure** &ndash; o trabalho não pôde ser agendado. Isso geralmente é causado por parâmetros de `JobInfo` conflitantes.

Esse código é um exemplo de agendamento de um trabalho e notificação do usuário sobre os resultados da tentativa de agendamento:

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
    snackBar.Show();
}
else
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
    snackBar.Show();
}
```

### <a name="cancelling-a-job"></a>Cancelando um trabalho

É possível cancelar todos os trabalhos que foram agendados ou apenas um único trabalho usando o método `JobsScheduler.CancelAll()` ou o método `JobScheduler.Cancel(jobId)`:

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>Resumo

Este guia abordou como usar o Agendador de trabalhos do Android para executar o trabalho de forma inteligente em segundo plano. Ele abordou como encapsular o trabalho a ser executado como um `JobService` e como usar o `JobScheduler` para agendar esse trabalho, especificando os critérios com uma `JobTrigger` e como as falhas devem ser tratadas com um `RetryStrategy`.

## <a name="related-links"></a>Links relacionados

- [Agendamento de trabalhos inteligentes](https://developer.android.com/topic/performance/scheduling.html)
- [Referência da API do JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [Agendando trabalhos como um pro com JobScheduler](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Otimizações de memória e bateria do Android – Google I/O 2016 (vídeo)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [JobScheduler Android – René Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
