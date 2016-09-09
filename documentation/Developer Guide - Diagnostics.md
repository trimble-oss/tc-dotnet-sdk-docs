# Trimble.Diagnostics Developer Guide


Trimble.Diagnostics component provides cross platform tools for code level tracing with TraceSource. 

It uses a similar approach for code instrumentation and tracing as the .NET network tracing (see [https://msdn.microsoft.com/en-us/library/ty48b824(v=vs.110).aspx](https://msdn.microsoft.com/en-us/library/ty48b824(v=vs.110).aspx)).

On desktop platforms, in order to enable tracing for e.g. **Trimble.Identity** trace source, please include the following in your app.config:
 
    <system.diagnostics>
    <sources>
        <source name="**_Trimble.Identity_**"></source>
    </sources>

    <switches>
      <add name="**_Trimble.Identity_**" value="Verbose"/>
    </switches>
    </system.diagnostics>

The following table shows what tracing messages are enabled depending on the value of *SourceLevels*.  The items in the leftmost column are the *SourceLevels* that you specify in the config.  The items in the header row represent the _TraceEventType_ values tied to a particular event.  A check mark (v) indicates that this level event will log against this *SourceLevel*.

<table>
  <tr>
    <td></td>
    <td>Critical</td>
    <td>Error</td>
    <td>Warning</td>
    <td>Information</td>
    <td>Verbose</td>
    <td>Start</td>
    <td>Stop</td>
    <td>Suspend</td>
    <td>Resume</td>
    <td>Transfer</td>
  </tr>
  <tr>
    <td>Off</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
  </tr>
  <tr>
    <td>Critical</td>
    <td>v</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
  </tr>
  <tr>
    <td>Error</td>
    <td>v</td>
    <td>v</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
  </tr>
  <tr>
    <td>Warning</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
  </tr>
  <tr>
    <td>Information</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
  </tr>
  <tr>
    <td>Verbose</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
  </tr>
  <tr>
    <td>ActivityTracing</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>x</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
  </tr>
  <tr>
    <td>All</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
    <td>v</td>
  </tr>
</table>


### Code tracing on mobile platforms

On mobile platforms (iOS, Android), app.config level tracing configuration is not yet available.  To configure tracing on these platforms please use the _Trimble.Diagnostics.Logger.Switch_ property. By default, tracing is switched off (SourceLevels.Off).


## Code Example
Below is a typical code example how to use diagnostics tools in other component. 

    /// <summary>
    /// The logging configuration.
    /// </summary>
    public static class Logging
    {
        /// <summary>
        /// The trace source name.
        /// </summary>
        private const string TraceSourceName = "Trimble.Identity";

        /// <summary>
        /// The flag whether logging is enabled.
        /// </summary>
        private static readonly Logger Instance = new Logger(TraceSourceName);

        /// <summary>
        /// Gets a value indicating whether logging is on.
        /// </summary>
        /// <value>The true if logging is on.</value>
        internal static Logger Logger
        {
            get
            {
                return Instance;
            }
        }

    #if __ANDROID__ || __IOS__
        /// <summary>
        /// Gets a trace switch that allow to configure level of code tracing.
        /// </summary>
        /// <value>The trace switch.</value>
        public static SourceSwitch Switch
        {
            get
            {
                return Logger.Switch;
            }
        }
    #endif
    } 

This class need to be in the public interface of the component so the mobile clients can control the logging level:

    Trimble.Identity.Logging.Switch.Level = SourceLevels.Verbose;
