diff --git a/Gruntfile.js b/Gruntfile.js
index c0b7c1a..aa13411 100644
--- a/Gruntfile.js
+++ b/Gruntfile.js
@@ -392,7 +392,7 @@ module.exports = function (grunt) {
       'clean:server',
       'wiredep',
       'concurrent:server',
-      'autoprefixer',
+      //'autoprefixer',
       'connect:livereload',
       'watch'
     ]);
diff --git a/app/concept1/SPA/controller.js b/app/concept1/SPA/controller.js
index 82ce9b2..d2e83bd 100644
--- a/app/concept1/SPA/controller.js
+++ b/app/concept1/SPA/controller.js
@@ -45,29 +45,40 @@ app.controller('Concept1Ctrl', ['$scope', '$interval', 'showerSettingModel', fun
 
     var checkTempreture = function(){
       var setting = $scope.setting;
-      if (setting !== null && currentTempt < setting.tempreture){
-        heatWater();
+
+      if(setting == null){
+        return;
+      }
+
+      // If the currentTemp is less than the preset, we need to heat the water
+      var presetTemperature = setting.tempreture;
+      if (currentTempt < presetTemperature){
+        heatWaterToPreset(presetTemperature);
       }
-      $interval.cancel(intervalPromise);
     }
 
-    var heatWater = {
-        start: $interval(function(){
+    var heatWaterToPreset = function(presetTemperature){
+        $scope.isHeating = true;
+        var intervalVar = $interval(function(){
+          // Increment temperature
           currentTempt += 1;
-          console.log(currentTempt);
-        },1000);
-      }
 
-    }
+          // When the temperature hit the preset, stop heating
+          if(currentTempt >= presetTemperature){
+            $interval.cancel(intervalVar);
+            $scope.isHeating = false;
+          }
+        }, 1000)
+      }
     // var stopHeating = function(){
     //   $interval.cancel(intervalPromise);
     //   //heatWater = undefined;
     // }
 
 
-    $scope.$on('$destroy', function(){
-      // Make sure that the interval is destroyed too
-      $interval.cancel(intervalPromise);
-    })
+    // $scope.$on('$destroy', function(){
+    //   // Make sure that the interval is destroyed too
+    //   $interval.cancel();
+    // })
 
   }]);
