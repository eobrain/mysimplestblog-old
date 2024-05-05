---  
slug: mtd75bqc8l
title: What Twitter scrubbed from its code
created: 2023-04-01 04:41:37.551415646+00:00
---  
<style>article{max-width: none}</style>

```
$ diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/ the-algorithm-main/
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/follow-recommendations-service/server/src/main/scala/com/twitter/follow_recommendations/services/FollowRecommendationsServiceWarmupHandler.scala the-algorithm-main/follow-recommendations-service/server/src/main/scala/com/twitter/follow_recommendations/services/FollowRecommendationsServiceWarmupHandler.scala
27,30d26
<   /**
<    * this would need to be added to src/main/resources/client_whitelist.yml
<    * if we implement ClientId filtering in the future
<    */
51c47
<         displayContext = Some(DisplayContext.Profile(Profile(12L))),
---
>         displayContext = None,
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/graph-feature-service/src/main/scala/com/twitter/graph_feature_service/server/controllers/ServerController.scala the-algorithm-main/graph-feature-service/src/main/scala/com/twitter/graph_feature_service/server/controllers/ServerController.scala
28c28
<     // TODO(yqian): Disable updateCache after HTL switch to use PresetIntersection endpoint.
---
>     // TODO: Disable updateCache after HTL switch to use PresetIntersection endpoint.
38c38
<     // TODO(yqian): Refactor after HTL switch to PresetIntersection
---
>     // TODO: Refactor after HTL switch to PresetIntersection
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/graph-feature-service/src/main/scala/com/twitter/graph_feature_service/server/handlers/ServerGetIntersectionHandler.scala the-algorithm-main/graph-feature-service/src/main/scala/com/twitter/graph_feature_service/server/handlers/ServerGetIntersectionHandler.scala
31c31
<   // TODO(yqian): Track all the stats based on PresetFeatureType and update the dashboard
---
>   // TODO: Track all the stats based on PresetFeatureType and update the dashboard
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/graph-feature-service/src/main/scala/com/twitter/graph_feature_service/server/handlers/ServerWarmupHandler.scala the-algorithm-main/graph-feature-service/src/main/scala/com/twitter/graph_feature_service/server/handlers/ServerWarmupHandler.scala
4c4,6
< import com.twitter.graph_feature_service.thriftscala.EdgeType.{FavoritedBy, FollowedBy, Following}
---
> import com.twitter.graph_feature_service.thriftscala.EdgeType.FavoritedBy
> import com.twitter.graph_feature_service.thriftscala.EdgeType.FollowedBy
> import com.twitter.graph_feature_service.thriftscala.EdgeType.Following
6c8,9
< import com.twitter.graph_feature_service.thriftscala.{FeatureType, GfsIntersectionRequest}
---
> import com.twitter.graph_feature_service.thriftscala.FeatureType
> import com.twitter.graph_feature_service.thriftscala.GfsIntersectionRequest
10c13,14
< import javax.inject.{Inject, Singleton}
---
> import javax.inject.Inject
> import javax.inject.Singleton
18,25c22,23
<   private val testingAccounts: Array[Long] = {
<     Seq(
<       12L, //jack
<       21447363L, // KATY PERRY
<       42562446L, // Stephen Curry
<       813286L // Barack Obama
<     ).toArray
<   }
---
>   // TODO: Add the testing accounts to warm-up the service.
>   private val testingAccounts: Array[Long] = Seq.empty.toArray
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/graph-feature-service/src/main/scala/com/twitter/graph_feature_service/util/IntersectionValueCalculator.scala the-algorithm-main/graph-feature-service/src/main/scala/com/twitter/graph_feature_service/util/IntersectionValueCalculator.scala
111c111
<    * TODO(yaow): for now it only computes intersection size. Will add more feature types (e.g., dot
---
>    * TODO: for now it only computes intersection size. Will add more feature types (e.g., dot
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/HomeTweetTypePredicates.scala the-algorithm-main/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/HomeTweetTypePredicates.scala
223,246c223
<     ("served_in_threaded_conversation_module", _ => false),
<     (
<       "author_is_elon",
<       candidate =>
<         candidate
<           .getOrElse(AuthorIdFeature, None).contains(candidate.getOrElse(DDGStatsElonFeature, 0L))),
<     (
<       "author_is_power_user",
<       candidate =>
<         candidate
<           .getOrElse(AuthorIdFeature, None)
<           .exists(candidate.getOrElse(DDGStatsVitsFeature, Set.empty[Long]).contains)),
<     (
<       "author_is_democrat",
<       candidate =>
<         candidate
<           .getOrElse(AuthorIdFeature, None)
<           .exists(candidate.getOrElse(DDGStatsDemocratsFeature, Set.empty[Long]).contains)),
<     (
<       "author_is_republican",
<       candidate =>
<         candidate
<           .getOrElse(AuthorIdFeature, None)
<           .exists(candidate.getOrElse(DDGStatsRepublicansFeature, Set.empty[Long]).contains)),
---
>     ("served_in_threaded_conversation_module", _ => false)
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RequestQueryFeatureHydrator.scala the-algorithm-main/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RequestQueryFeatureHydrator.scala
3d2
< import com.twitter.config.yaml.YamlMap
9d7
< import com.twitter.home_mixer.param.HomeMixerInjectionNames.DDGStatsAuthors
27d24
< import javax.inject.Named
33,34c30
<   @Named(DDGStatsAuthors) ddgStatsAuthors: YamlMap)
<     extends QueryFeatureHydrator[Query] {
---
> ) extends QueryFeatureHydrator[Query] {
39,42d34
<     DDGStatsDemocratsFeature,
<     DDGStatsRepublicansFeature,
<     DDGStatsElonFeature,
<     DDGStatsVitsFeature,
62,65d53
<   private val Democrats = "democrats"
<   private val Republicans = "republicans"
<   private val Elon = "elon"
<   private val Vits = "vits"
86,95d73
<       /**
<        * These author ID lists are used purely for metrics collection. We track how often we are
<        * serving Tweets from these authors and how often their tweets are being impressed by users.
<        * This helps us validate in our A/B experimentation platform that we do not ship changes
<        * that negatively impacts one group over others.
<        */
<       .add(DDGStatsDemocratsFeature, ddgStatsAuthors.longSeq(Democrats).toSet)
<       .add(DDGStatsRepublicansFeature, ddgStatsAuthors.longSeq(Republicans).toSet)
<       .add(DDGStatsVitsFeature, ddgStatsAuthors.longSeq(Vits).toSet)
<       .add(DDGStatsElonFeature, ddgStatsAuthors.longValue(Elon))
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/HomeFeatures.scala the-algorithm-main/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/HomeFeatures.scala
179,182d178
<   object DDGStatsElonFeature extends Feature[PipelineQuery, Long]
<   object DDGStatsVitsFeature extends Feature[PipelineQuery, Set[Long]]
<   object DDGStatsDemocratsFeature extends Feature[PipelineQuery, Set[Long]]
<   object DDGStatsRepublicansFeature extends Feature[PipelineQuery, Set[Long]]
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/HomeMixerResourcesModule.scala the-algorithm-main/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/HomeMixerResourcesModule.scala
3,5d2
< import com.google.inject.Provides
< import com.twitter.config.yaml.YamlMap
< import com.twitter.home_mixer.param.HomeMixerInjectionNames.DDGStatsAuthors
7,8d3
< import javax.inject.Named
< import javax.inject.Singleton
10,18c5
< object HomeMixerResourcesModule extends TwitterModule {
< 
<   private val AuthorsFile = "/config/authors.yml"
< 
<   @Provides
<   @Singleton
<   @Named(DDGStatsAuthors)
<   def providesDDGStatsAuthors(): YamlMap = YamlMap.load(AuthorsFile)
< }
---
> object HomeMixerResourcesModule extends TwitterModule {}
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeMixerInjectionNames.scala the-algorithm-main/home-mixer/server/src/main/scala/com/twitter/home_mixer/param/HomeMixerInjectionNames.scala
7d6
<   final val DDGStatsAuthors = "DDGStatsAuthors"
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/framework/full_type.proto the-algorithm-main/navi/navi/proto/tensorflow/core/framework/full_type.proto
125c125
<   // TODO(mdan): Define TFT_SHAPE and add more examples.
---
>   // TODO: Define TFT_SHAPE and add more examples.
181c181
<   // TODO(mdan): Quantized types, legacy representations (e.g. ref)
---
>   // TODO
198c198
<   // TODO(mdan): Represent as TFT_COMPLEX[TFT_DOUBLE] instead?
---
>   // TODO: Represent as TFT_COMPLEX[TFT_DOUBLE] instead?
243c243
<   // TODO(mdan): Properly document this thing.
---
>   // TODO: Properly document this thing.
274c274
<     // TODO(mdan): list/tensor, map? Need to reconcile with TFT_RECORD, etc.
---
>     // TODO: list/tensor, map? Need to reconcile with TFT_RECORD, etc.
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/framework/function.proto the-algorithm-main/navi/navi/proto/tensorflow/core/framework/function.proto
26c26
< // TODO(zhifengc):
---
> // TODO:
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/framework/node_def.proto the-algorithm-main/navi/navi/proto/tensorflow/core/framework/node_def.proto
64c64
<   // TODO(josh11b): Add some examples here showing best practices.
---
>   // TODO: Add some examples here showing best practices.
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/framework/op_def.proto the-algorithm-main/navi/navi/proto/tensorflow/core/framework/op_def.proto
99c99
<     // TODO(josh11b): bool is_optional?
---
>     // TODO: bool is_optional?
142c142
<   // TODO(josh11b): Implement that optimization.
---
>   // TODO: Implement that optimization.
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/framework/step_stats.proto the-algorithm-main/navi/navi/proto/tensorflow/core/framework/step_stats.proto
56c56
<   // TODO(tucker): Use some more compact form of node identity than
---
>   // TODO: Use some more compact form of node identity than
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/framework/tensor.proto the-algorithm-main/navi/navi/proto/tensorflow/core/framework/tensor.proto
19c19
<   // Shape of the tensor.  TODO(touts): sort out the 0-rank issues.
---
>   // Shape of the tensor.  TODO: sort out the 0-rank issues.
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/protobuf/config.proto the-algorithm-main/navi/navi/proto/tensorflow/core/protobuf/config.proto
535c535
<     // TODO(shikharagarwal): Should we just remove this tag so that it can be
---
>     // TODO: Should we just remove this tag so that it can be
579c579
<     // TODO(b/129330037): Add a single API that consistently treats
---
>     // TODO: Add a single API that consistently treats
707c707
<   // TODO(pbar) Turn this into a TraceOptions proto which allows
---
>   // TODO Turn this into a TraceOptions proto which allows
784c784
<     // TODO(nareshmodi): Include some sort of function/cache-key identifier?
---
>     // TODO: Include some sort of function/cache-key identifier?
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/protobuf/coordination_service.proto the-algorithm-main/navi/navi/proto/tensorflow/core/protobuf/coordination_service.proto
197c197
<   // TODO(b/195990880): Consider splitting this into a different RPC service.
---
>   // TODO: Consider splitting this into a different RPC service.
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/protobuf/debug_event.proto the-algorithm-main/navi/navi/proto/tensorflow/core/protobuf/debug_event.proto
15c15
< // TODO(cais): Document the detailed column names and semantics in a separate
---
> // TODO: Document the detailed column names and semantics in a separate
226c226
<   // TODO(cais): Test the uniqueness guarantee in multi-host settings.
---
>   // TODO: Test the uniqueness guarantee in multi-host settings.
267c267
<   // TODO(cais): When backporting to V1 Session.run() support, add more fields
---
>   // TODO support, add more fields
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/protobuf/debug.proto the-algorithm-main/navi/navi/proto/tensorflow/core/protobuf/debug.proto
49c49
<   // TODO(cais): More visible documentation of this in g3docs.
---
>   // TODO: More visible documentation of this in g3docs.
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/protobuf/distributed_runtime_payloads.proto the-algorithm-main/navi/navi/proto/tensorflow/core/protobuf/distributed_runtime_payloads.proto
10c10
< // TODO(b/204231601): Use GRPC API once supported.
---
> // TODO: Use GRPC API once supported.
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/protobuf/eager_service.proto the-algorithm-main/navi/navi/proto/tensorflow/core/protobuf/eager_service.proto
175c175
<   // TODO(nareshmodi): Consider adding NodeExecStats here to be able to
---
>   // TODO: Consider adding NodeExecStats here to be able to
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/protobuf/master.proto the-algorithm-main/navi/navi/proto/tensorflow/core/protobuf/master.proto
97c97
<   // TODO(mrry): Return something about the operation?
---
>   // TODO: Return something about the operation?
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/protobuf/saved_object_graph.proto the-algorithm-main/navi/navi/proto/tensorflow/core/protobuf/saved_object_graph.proto
179c179
<   // TODO(b/169361281): support calling saved ConcreteFunction with structured
---
>   // TODO: support calling saved ConcreteFunction with structured
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/protobuf/tensor_bundle.proto the-algorithm-main/navi/navi/proto/tensorflow/core/protobuf/tensor_bundle.proto
20c20
< // TODO(zongheng,zhifengc): maybe in the future, we can add information about
---
> // TODO: maybe in the future, we can add information about
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow/core/protobuf/worker.proto the-algorithm-main/navi/navi/proto/tensorflow/core/protobuf/worker.proto
191c191
<   // TODO(mrry): Optionally add summary stats for the graph.
---
>   // TODO: Optionally add summary stats for the graph.
297c297
<   // TODO(suharshs): Package these in a RunMetadata instead.
---
>   // TODO: Package these in a RunMetadata instead.
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow_serving/apis/logging.proto the-algorithm-main/navi/navi/proto/tensorflow_serving/apis/logging.proto
16c16
<   // TODO(b/33279154): Add more metadata as mentioned in the bug.
---
>   // TODO: Add more metadata as mentioned in the bug.
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow_serving/config/file_system_storage_path_source.proto the-algorithm-main/navi/navi/proto/tensorflow_serving/config/file_system_storage_path_source.proto
61c61
<   // TODO(b/30898016): Stop using these fields, and ultimately remove them here.
---
>   // TODO: Stop using these fields, and ultimately remove them here.
79c79
<   // TODO(b/30898016): Remove 2019-10-31 or later.
---
>   // TODO: Remove 2019-10-31 or later.
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/navi/navi/proto/tensorflow_serving/config/model_server_config.proto the-algorithm-main/navi/navi/proto/tensorflow_serving/config/model_server_config.proto
12c12
< // TODO(b/31336131): DEPRECATED.
---
> // TODO: DEPRECATED.
34c34
<   // TODO(b/31336131): DEPRECATED. Please use 'model_platform' instead.
---
>   // TODO: DEPRECATED. Please use 'model_platform' instead.
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/product-mixer/component-library/src/main/scala/com/twitter/product_mixer/component_library/model/candidate/suggestion/QuerySuggestionCandidate.scala the-algorithm-main/product-mixer/component-library/src/main/scala/com/twitter/product_mixer/component_library/model/candidate/suggestion/QuerySuggestionCandidate.scala
234c234
<  * TODO(jhara) Remove score from the candidate and use a Feature instead
---
>  * TODO Remove score from the candidate and use a Feature instead
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/README.md the-algorithm-main/README.md
13c13
< | Feature | [simclusters-ann](simclusters-ann/README.md) | Community detection and sparse embeddings into those communities. |
---
> | Feature | [SimClusters](src/scala/com/twitter/simclusters_v2/README.md) | Community detection and sparse embeddings into those communities. |
diff -r the-algorithm-7f90d0ca342b928b479b512ec51ac2c3821f5922/src/scala/com/twitter/recos/graph_common/BipartiteGraphHelper.scala the-algorithm-main/src/scala/com/twitter/recos/graph_common/BipartiteGraphHelper.scala
11d10
<  * TODO (wenqih) change TweetIDMask to a mask interface for future extension
```